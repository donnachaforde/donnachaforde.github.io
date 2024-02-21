[home/](../../../)[blogs+posts/](../../)[software-engineering/](../)[design-considerations](./design-considerations)

# Design Considerations
### Considerations and Influences on the Design of the 'espresso' Library


### Introduction

The idea of building a library to handle command-line arg parsing came to me many years ago while I was still a relatively junior engineer. I'd been working on a migration feature that needed to operate within a strictly limited time-window and I needed to experiment to determine the optimum number of concurrent threads to use. I'd made use of multithreading to maximize throughput and was running extensive tests to find that optimal configuration when having the convenience of externalizing this parameter became abundantly clear. So, I implemented a rudimentary arg parsing solution for the task at hand but a curiosity for a more robust solution was ignited and so it began... 

I figured it would have general applicability so a standalone library seemed apt. Some time later, having gained experience of API design and Physical Code Design, I remodeled the library into pretty much the current layout. 

At this point, anytime I had an idea for a handy tool or utility, I was able to concentrate on the task and not have to worry about handling the usual options for help, version and usage information, etc. 



### Design Goals/Influences

There are a few stylistic aspects to the solution that are a result of some influences: 

* I like libraries that have a single header for simple inclusion and adoption. 
* Equally, I like libraries that are logically segregated so you can include the parts you like and avoid having to `#include` the parts you don't. 
* I wanted a cross-platform  solution so I could build tools for Windows and UNIX/Linux. Later, as macOS emerged as a development platform, I was able to port to this OS. 
* Internally, I wanted to take advantage of precompiled headers when working on Windows. 
* I wanted a completely clean build, without any compiler or linker warnings so I wanted to be able to disable some compiler warnings. However, I wanted a way for the user to disable that.
* I like to use write additional messages to the output window when compiling, but I wanted to provide a way to disable this in case they annoyed other users. 

So, as theme, I wanted ultimately flexibility. 


### Design Evolution

When working with C++ and taking an object-oriented approach, there are lots of ways to devise a solution. It occurred to me that while I originally developed the library for use with CMD-line tools, many Windows developers have a preference for using the Windows UI. So, while you may well launch the command from the CMD-line with optional arguments, it actually launches a Windows GUI. This really challenged my assumptions about the library writing to `stdout`. I didn't want to limit scope so I set about separating the logic used to parse arguments from the logic used to write to `stdout`. 

My initial attempt was to use class inheritance to separate 'common' logic into an abstract base class and pushing the parts leveraging `stdout` into a concrete class. This succeeded in achieving the design goal but it didn't sit well with me as an elegant solution. 

Later, when working with Java and learning about Spring's Inversion of Control (IOC) design pattern, I gained a deeper understanding of separating functionality down to the core elements. In this case, I realized that rendering output is quite distinct from argument parsing and by separating the interface from the implementation, it allowed me to provide a default `stdout` renderer but still leave it possible for other developers to devise their own implementation. 

I've detailed that design journey in a separate article on [object construction](./object-construction). 



### Style, Conventions and External Influences

When I first devised the espresso library, I was strongly influenced by the Visual C++ naming conventions, using CamelCase (with a leading capital letter). I always thought this more aesthetically pleasing that the use of underscores in names, commonly_seen_in UNIX systems programming circles. However, after many years of Java programming, I came round to having a preference for camelCase (without' the leading uppercase letter). 

Also, I've was strongly influenced by the book [Writing Solid Code](https://www.amazon.co.uk/Writing-Solid-Code-Techniques-Programming/dp/1556155514) _by Steve Maguire_ early on in my career where [Hungarian notation](https://en.wikipedia.org/wiki/Hungarian_notation) was explained and I've been using a 'light' version ever since. 


### IDE History

This library was first developed using Visual C++ 6.0. That version of the Microsoft C/C++ compiler had quite a long life and the project was still tied to that version for many years. Eventually, it was upgraded to Visual Studio 2003 (Visual C++ 7), then Visual C++ 8 and Visual C++ 9. Thereafter, it was neglected while I pursued pure Java development and didn't get any attention until Visual Studio 2014 but then was upgraded to Visual Studio 17 and most recently Visual Studio 2019. 


***
_Donnacha Forde_

_espresso library - 1995-2021_

_[linkedIn.com/in/donnachaforde](https://www.linkedin.com/in/donnachaforde)_
