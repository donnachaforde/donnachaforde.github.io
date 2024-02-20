[home/](../../../)[blogs+posts/](../../)[software-engineering/](../)[antipattern-stl-inheritance](./)


# Design Antipattern - STL Inheritance


<img src="https://upload.wikimedia.org/wikipedia/commons/1/18/ISO_C%2B%2B_Logo.svg" width="300" height="300" >

_Photo Credit: [wikimedia](https://upload.wikimedia.org/wikipedia/commons/1/18/ISO_C%2B%2B_Logo.svg)_

### Never, Ever Inherit from an STL Container

That is unless you're following the style and convention of the STL library to create a template class to represent a modified form of container. Otherwise, you're most probably applying an antipattern to your design and you'll likely end up with unexpected behaviour, resulting in memory leaks. 

Why? 

Well, because it's wrong on both of two very important points: 1) **Design** and 2) **Implementation**. 

#### Design

If you find yourself tempted to inherit from an STL container because your class represents a list or some other grouping, then you might be violating the design difference between 'is a' and 'has a'. Just because the class you're building represents a collection of things doesn't mean it 'is a' collection of things. It more likely means that it 'has a' collection of things. _(See Item 32 and Item 38 in Effective C++ Third Edition by Scott Meyers)_

For example, if you consider your class as representing a list of things, then you likely needs to contain a `vector<thing>` things rather than inheriting from it.

The following code represents a 'is a' design. 
````C++
class Foo : public vector<thing>
{
    // asking for trouble
    .
    .
    .
};
````

Whereas the following represents a 'has a' design.
````C++
class Foo
{
private:
    // 'has a'
    vector<thing> myListofThings;

    .
    .
    .
};
````


#### Implementation

The STL was not designed to be extended through normal OO class inheritance. Apart from the fact that the STL represents _template classes_ rather than actual concrete classes, the most important factor is that the STL was designed with performance in mind from the outset and intentionally excluded _virtual functions_ in its implementation. That includes _virtual destructors_,  which means that if you inherit from a container, you're not going to achieve the correct order of object destruction you might be depending on to release heap memory and other resources, resulting in memory leaks and dangling resources. 


#### Summary
One of the worst examples of a memory leak I ever had to deal with in my career was because of this _antipattern_. I was given the task of converting a batch system into a real-time service and once the system was developed, a catastrophic memory leak in the underlying libraries became apparent. It'd never been noticed in the batch system because those libraries were exercised only once before the process terminated whereas now, they were embedded in an online, always-on service, which meant they'd be exercised over-and-over without the process terminating (and memory being released). 

Actually, getting to that point took effort in itself because the server crashed immediately upon innovation, which meant the issue couldn't be debugged in a debugger. It took some ol' fashioned crash analysis by-hand to determine the cause of the crash might actually be a lack of available memory. And, in turn, that that had been caused by earlier leaks. Further still, it wasn't just a handful of leaks but rather a collection of thousands of minor leaks that collectively added up to a massive leak (relative to the typical machine specs of the time). 


Unfortunately, the _antipattern_ had been applied liberally, which meant there were several classes affected and in turn, associated references (e.g. _iterators_, etc.) were rampant throughout the codebase. That meant that, having diagnosed the problem, the majority of the work had only began because so much of the codebase was affected. 

In order to cope with the extent of the disruption and minimize impact, I modelled constructs like `iterator` and `const_iterator` as well as methods like `begin()` and `end()` within the classes themselves. So began iteration after iteration of breaking the class inheritance in order to have the compiler let me know of all the broken references. After several weeks of this painstaking work, I eventually managed to get the issue under control and the server could actually function. 

The point is that all of this rework could have been avoided had the original author simply known that the STL containers aren't designed to be extended through inheritance. 


***
_Donnacha Forde_

_[linkedIn.com/in/donnachaforde](https://www.linkedin.com/in/donnachaforde)_




