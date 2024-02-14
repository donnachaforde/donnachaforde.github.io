[home/](../../../)[blogs+posts/](../../)[software-engineering/](../)[object-construction](./object-construction)


# Object Construction
The thought process around object construction when using the espresso library constructs. 


## Introduction

In this blog, I explore construction patterns for initializing the library. As described in [ design influences and considerations](./design-considerations.md), my goal was to provide a way to initialize the library for writing to `stdout` without excluding alternative rendering. 



## Influences

When thinking about design, I find it helps to have a conversation with yourself to try to identify 'baggage' - what pre-conceptions and preferences you're bringing to the table on day-one.  

* I like simple and clean interfaces that are easy to use and encourage adoption.  
* I prefer Factory classes over constructors.
* I like utilizing 'Manager' classes or singletons where there is ever only one instance or there's is a need to address a set of objects collectively.
* I like clearly defined roles and responsibilities when it comes to memory management and resource usage. Consequently, I'm often reluctant to defer to using the heap when I don't control both client and server (or caller and callee) code. 
* I admire the IOC design pattern employed in Spring. 




## Design Evolution

Taking the programmer's experience of the library into consideration, I wanted a clear way to model and capture command-line argument info, as follows:

	Args args(...);

	args.add("option1");
	args.add("option2");
	args.add("option3");
	
	args.addAlias("option1", 'o');


Next, I wanted the user to declare a manager object that, under the covers, defined the relationship between the args and the arg-renderer. Early on, I let the user define the Argument-Manager directly but once I had incorporated the ability to render the args in more ways than `stdout`, a factory made more sense. 

Originally, the code would have had to be declared as follows:

	StdoutArgRenderer stdoutArgRenderer;
	ArgMgr argMgr(stdoutArgRenderer);

The idea being that if a developer wanted to show command-line argument info in a dialog box, they could define their own render and initialize as follows:

	WindowsArgRenderer windowsArgRenderer;
	ArgMgr argMgr(windowsArgRenderer);


This was possible because I had defined an interface for arg-rendering - i.e. `IArgRenderer` to allow the user to provide their own renderer. The `ArgMgr` class holds a reference of type `IArgRenderer` and in turn, the object delegates all print output to this instance. So, the trick is simply to provide the ArgMgr instance with a valid ArgRenderer. 


However, I really wanted this to encapsulate this detail for the vast majority of users so I decided to employ a Factory that would do this simple initialization for me. 

	ArgManager argMgr = ArgManagerFactory::createInstance();

Under the covers, an early attempt took this approach:

	ArgMgr ArgManagerFactory::createInstance()
	{
		StdoutArgRenderer stdoutArgRenderer;
		return createInstance(stdoutArgRenderer);
	}

	ArgMgr ArgManagerFactory::createInstance(IArgRenderer& argRenderer)
	{
		ArgMgr argMgr(argRenderer);
		return argMgr;
	}
The mechanics of this approach meant that I had to provide a copy-constructor implementation for ArgMgr, though this was trivial. You can see that the cleanest implementation for the factory methods involved passing back a copy of the object rather than a pointer or reference (thus avoiding any messy responsibilities for deletion). 


Of course, this approach didn't work. 


Even though this succeeded in providing the `ArgMg` object with a valid reference to a renderer object, declaring the `stdoutArgRenderer` on the heap meant that it got destructed when the factory method returned. The valid reference referenced invalid memory causing an Access Violation. 

Incidentally, things work when the user explicitly declares the ArgRenderer (as above) because it will still exist on the heap. 

My first reaction was that, despite my best efforts, I was going to have to rely on the renderer object being allocated on the heap. I couldn't utilize concrete objects **and** rely on dynamic types at runtime. The `ArgMgr` class couldn't know the concrete type of the object it was interacting with here. It had to use a pointer or reference in order for that dynamic relationship. In this instance, it's clear that the GC approach deployed in Java provides a greater degree of freedom in facilitating Dynamic Injection (DI) and the Inversion of Control (IoC) pattern. 

Frustrated that I was going to be forced to rewrite the code to use dynamically allocated objects (on the heap) and annoyed with all that entailed (See: Item 27 in _More Effective C++ by Scott Meyers_), I just took a break from it... 
	

### Eureka

I've read several books on engineering productivity and came across many references describing how people had a light-bulb moment while away from the keyboard. That's exactly what happened: it occurred to me that I could use the `static` keyword to declare the `stdout` renderer. 

Because of the way the language has evolved, C++ reuses keywords so, as of the time of writing, `static` had 3 distinct uses. But, when used within a function or method, it meant that instance was only created once and never destroyed. 

	ArgMgr ArgManagerFactory::createInstance()
	{
		static StdoutArgRenderer stdoutArgRenderer;
		return createInstance(stdoutArgRenderer);
	}

Simply declaring the `stdoutArgRenderer` as `static` meant the object effectively behaved as though it'd been provided by the user, further up the stack. 

Great! This worked a treat and meant that I was now able to meet my fussy design goals. The only minor annoyance being that I didn't think of it sooner... :-) 

***
_Donnacha Forde_

_May 2021_

_[linkedIn.com/in/donnachaforde](https://www.linkedin.com/in/donnachaforde)_
