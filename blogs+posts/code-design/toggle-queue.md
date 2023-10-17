[home/](https://donnachaforde.github.io)[blogs+posts/](https://donnachaforde.github.io/blogs+posts/)[code-design/](https://donnachaforde.github.io/blogs+posts/code-design/)[toggle-queue](./toggle-queue.md)

# Toggle Queue
A design pattern (in Java) for queue objects that's a little kinder to the Garbage Collector (GC).


## Introduction

A number of years ago, I was working in a role where I had special responsibility for Performance & Scalability. It basically gave me _carte blanche_ to poke around the entire codebase, which at approx. 2 million lines of code was substantial. The thing about performance tuning in general is that there most typically isn't the one or two obvious silver bullets that jump out at you but instead, hundreds if not thousands of smaller, seemingly innocuous issues to address. 

On this occasion, the profiler drew my attention to a small section of the code that was generating above-average work for the JVM Garbage Collector (GC). It was consistently allocating objects that the GC had to free. When I dug into it, I found that the code was effectively acting as an event-buffer that batched up collections of events and was forwarding them for downstream processing. T



## Background
Events arrived into the system asynchronously and were held in a buffer and then subsequently dispatched in a batch in a separate thread. Internally, a collection class was used to hold the references to the event objects and each time the dispatching thread fired, the collection was handed over. While the dispatcher-thread went about its business of handling  those events, a new collection object was allocated by the receiver-thread to be used to buffer events now arriving into the system. 

## Problem

The upshot is that this code section was continuously allocating new `ArrayLists` that subsequently had to be freed when the dispatcher thread was done with them. Now, I hasten to add that this was a relatively small amount of memory on each occasion but what struck me was that it would continuously do so for the entire lifetime of the process and this was a backend-server meant to run indefinitely. In other words, it make work for the GC to do for the lifetime of the server.

Additionally, given that the `ArrayList` was created using the default ctor, it allocated the default internal size and was reallocating memory as and when that default threshold was surpassed. In other words, not only was the `ArrayList` object itself being repeatedly allocated but the internal array used to hold the collection of object references was also generating work for the GC.  

## Analysis & Solutions
The primary objective was straightforward: stop repeatedly allocating memory. 

That meant fixing two issues:

1. Avoid reallocating the internal array of the `ArrayList`.
2. Figure out a way to avoid having to create new  `ArrayList` objects each time we toggled the queue. 


### Preallocating Memory Buffer
Regarding the first issue, allocating a sufficiently large enough internal memory buffer for the `ArrayList` in the first instance should avoid any subsequent reallocations. This of course would mean that a little more memory was allocated than might otherwise have been needed but the main objective here was to reduce the work done in GC interrupts. So, the trade off would be worth it.

>Note: The default constructor for the Java ArrayList<> creates an empty list with capacity for 10 elements. 

It's important to be aware and understand that Java Generics supports another constructor that allows you to specify the initial capacity. From the [JDK javadocs](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#ArrayList--):



	public ArrayList()						// Constructs an empty list with an initial capacity of ten

	public ArrayList(int initialCapacity)  	// Constructs an empty list with the specified initial capacity.


For those unfamiliar with the internal operation of the Collection Classes, such as `ArrayList<>`, these classes/generics work by allocating an internal array of `<E>` elements. Arrays are fixed length so as this size becomes exceeded, the object allocates a new, larger array, copies over the contents of the old array into the new array and finally, discards the old array. The _rate of growth_ of these internal memory buffers is arguably conservative so if you're dealing with large numbers, especially if they're increasing dynamically, then it's much more efficient to simply allocate an `ArrayList<>` with an initial capacity. 

Also, when you consider it, this is a relatively expensive operation when you're adding elements to a collection in a tight loop. So, avoiding this additional work would also yield marginal gains. 

In this case, we knew size of the fixed-length internal buffer of the component publishing these events so that became our default constant. 

	List<T> queue = new ArrayList<T>(MAX_SIZE_OF_QUEUE);

Taking this simple step ensured the first issue was addressed and we'd never need to incur a realloc when adding events to our internal queue. 

### Avoiding New Memory Allocations
The second part would prove a little more challenging... How could we avoid having to allocate the new `ArrayList<>` object every time the dispatcher-thread triggered?

In the original implementation, each interrupt by the dispatcher-thread involved that thread taking ownership of the internal queue. The receiver-thread would receive events asynchronously, add them to the queue and then, when the dispatcher-thread signaled, hand over ownership of the queue and go on to create a new queue instance to use for buffering until the next interrupt/signal. 

This was one instance when stepping away from the keyboard helped greatly. Rather than viewing the scenario like some form of iterative behaviour, it occurred to me that it was more like a switch - i.e. a flip/flop switch. Gaining this perspective was key to the ultimate solution, which was a **Toggle Queue**, which is simply an object that contains two permanent queues - one that is currently being used by the dispatcher-thread and one that is being used by the receiver-thread. The model in this case is that the 'interrupt' acts as a flipping or flopping action. It causes the queues to be swapped (in a thread-safe way). 

To encapsulate this concept, I defined a new generic class, which I called a `ToggleQueue<T>`. 

### Implementation Details
The data internals need two queues (lists) and a further reference to indicate the current container. 


    // internal ref to the current queue
    private List<T> queue = null;

    // internal ref to first queue
    private List<T> flip = null;

    // internal ref to second queue
    private List<T> flop = null;


The constructor looked after the initialization of the two queues (lists), as follows:

  	public ToggleQueue(int size)
    {
        flip = Collections.synchronizedList(new ArrayList<T>(size));
        flop = Collections.synchronizedList(new ArrayList<T>(size));
        
		queue = flip;
    }

>Note: Thread-safe instances of the ArrayList are created.


Now, this approach uses double the memory size as before but we must remember a) our objective was to eliminate work for the GC and b) we're talking about containers that are merely holding references to event objects. In other words, in this case, we're not talking about large volumes of memory. 

Remember that in this instance a single thread is responsible for adding events to the container, which it does by invoking the `addAll()` method. The implementation simply adds the list of events passed in to the (current) internal list. 

   	public void addAll(Collection<T> events)
    {
        synchronized (this)
        {
            queue.addAll(events);
            notify();
        }
    }


The smart part happens in the method to `getAll()` events from the ToggleQueue. Here, it must make a copy of the current queue/list for the calling thread but before that, it needs to toggle to queues. 

	synchronized (this)
	{
		.
		. 
		.
		currentQueue = m_queue;

		// toggle the internal container
		isToggled = !isToggled;
		queue = (isToggled ? flip : flop);

		.
		.
		.
	}

Once we've flicked the flip/flop switch, we're free to copy the buffered events and clear the queue so as to reuse it for the next iteration. The reference to `currentQueue` now points at the toggled-off list. However, we still need to protect our access to it to avoid any potential race-condition whereby it could become the active queue before we've finished copying over the contents...

	List<T> result = Collections.emptyList();
	if (!currentQueue.isEmpty())
	{
		result = new ArrayList<T>(currentQueue);
		currentQueue.clear();
	}

## Summary

***
Donnacha Forde

_October, 2023_

_[linkedIn.com/in/donnachaforde](https://www.linkedin.com/in/donnachaforde)_