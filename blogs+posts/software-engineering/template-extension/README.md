[home/](../../../)[blogs+posts/](../../)[software-engineering/](../)[template-extension](./)


## A Note on Template Extension

<img src="https://upload.wikimedia.org/wikipedia/commons/1/18/ISO_C%2B%2B_Logo.svg" width="300" height="300" >

_Photo Credit: [wikimedia](https://upload.wikimedia.org/wikipedia/commons/1/18/ISO_C%2B%2B_Logo.svg)_

In C++, _class_ and _template class_ are different things, which show up in the way that each are used to model the problem domain. I like to use language like _"you inherit from a class"_ but _"you extend a template"_. Sure, both are physically extending the thing but the effect is that you expand the function of a class whereas you restrict the function of a template (at least in the STL). 

In typical OO design, you start with an _interface_ and perhaps an _abstract class_ so that you may generalize and rely on polymorphism. For example, if I were modelling shapes, I might have something like the following:

````C++
interface IShape
{
    void draw(); 
    void print(); 
};

interface ICircle : public IShape
{
    double getRadius();
};


class Circle : public ICircle
{
    ...
};

````

In broad terms, you extend the functionality of the parent. You typically extend the interface by adding to it, thus introducing new behaviour. 

However, the design of the STL could almost be described as the inverse of this. The topmost templates contain the fullest functionality and the 'inherited' templates effectively restrict functionality. Technically, inheritance isn't actually used but rather 'implemented in terms of' (often modelled in C++ as _private inheritance_).

A good example of this is the implementation of the `queue<>` template, which is implemented in terms of a `deque<>` - i.e. a 'double-ended queue'. This makes sense in so much as a `queue<>` is a restricted form of a `deque<>`. Whereas it's possible to traverse a `deque<>` in either direction, a `queue<>` can only be traversed in one direction, hence it's a 'queue'. 

Here's the STL code declaring `queue<>`:

````C++
template <class T, class _Container = deque<T>>
class queue 
{
    .
    .
    .

protected:
    _Container c{};
};    
````
Note the second template argument, which defaults to `deque<T>`, is used to declare the only member variable in `class Queue<T>` and is what's used to provide the implementation of `queue<T>`. **The queue is implemented in terms of deque.** 



If you set out to model this sort of construct in a Object-Oriented class library (which was popular in the '90s with libraries like RougeWave), you'd probably design it along the lines of first defining `class queue` containing elements with a single pointer to the next element and then extend it through inheritance to define `class deque` with an additional pointer to the previous element. But, in generic or template design, you invert that approach and define `template <T> class deque` first and then define `template <T> class queue` as a restricted form of `deque`.

This illustrates an important way Object-oriented design differs from 'Generic' design. Remember, C++ can support multiple languages and paradigms and that what we have today in C++ development is arguably a _hotpotch_ of technology and paradigms with a mix of OO and Generics. 


***
_Donnacha Forde_

_[linkedIn.com/in/donnachaforde](https://www.linkedin.com/in/donnachaforde)_




