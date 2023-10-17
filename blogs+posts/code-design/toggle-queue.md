[home/](https://donnachaforde.github.io)[blogs+posts/](https://donnachaforde.github.io/blogs+posts/)[code-design/](https://donnachaforde.github.io/blogs+posts/code-design/)[toggle-queue](./toggle-queue.md)

# Toggle Queue
A design pattern (in Java) for queue objects that's a little kinder to the Garbage Collector (GC).


## Introduction

A number of years ago, I was working in a role where I had special responsibility for Performance & Scalability. It basically gave me _carte blanche_ to poke around the entire codebase, which at approx. 2 million lines of code was substantial. The thing about performance tuning in general is that there most typically isn't the one or two obvious silver bullets that jump out at you but instead, hundreds if not thousands of smaller, innocuous issues to address. 

On this occasion, the profiler drew my attention to a small section of the code that was generating above-average work for the JVM Garbage Collector. It was consistently allocating objects that the GC had to free. When I dug into it, I found that the code was effectively acting as an event-buffer that batched up collections of events and forwarding them for downstream processing. 



## Background
Events arrived into the system asynchronously and were held in a buffer and then subsequently dispatched in a batch in a separate thread. Internally, a collection class was used to hold the references to the event objects and each time the dispatching thread fired, the collection was handed over. While the dispatching thread handled those events, a new collection object was allocated by the receiving thread that was used to buffer events now arriving into the system. 

## Problem

The upshot is that the code section was continuously allocating new `ArrayLists` that subsequently had to be freed when the dispatcher thread was done with them. Now, I hasten to add that this was a relatively small amount of memory on each occasion but what struck me was that it would continuously do this for the entirety of the lifetime of the process and this was a backend server that was meant to run forever. 

Additionally, given that the ArrayList was created using the default ctor, it allocated the default internal size and was reallocating memory as and when that default threshold was surpassed. In other words, not only was the ArrayList object itself being repeatedly allocated but the internal array used to hold the collection of object references was also generating work for the GC.  

## Solution
The primary objective was straightforward: stop repeatedly allocating memory. Allocating a sufficiently large enough buffer in the first instance would mean that the internal memory buffer of the ArrayList never should mean would never have to be extended but what about the collection object itself? Could we avoid having to allocate a new object every time the dispatcher thread triggered?



***
Donnacha Forde

_October, 2023_

_[linkedIn.com/in/donnachaforde](https://www.linkedin.com/in/donnachaforde)_