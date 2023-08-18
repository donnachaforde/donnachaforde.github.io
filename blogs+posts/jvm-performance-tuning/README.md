[home/](https://github.com/donnachaforde/donnachaforde.github.io)[blogs+posts/](https://donnachaforde.github.io/blogs+posts/)[jvm-performance-tuning](https://donnachaforde.github.io/blogs+posts/jvm-performance-tuning/)


# JVM Performance Tuning



A five-part blog on Java Virtual Machine Tuning, published on [blogger.com](https://donnachaforde.blogspot.com), covering:

+ [JVM Concepts](https://donnachaforde.blogspot.com/2015/09/jvm-performance-tuning-part-i-jvm.html)
+ [The Java Memory Model](https://donnachaforde.blogspot.com/2015/10/jvm-performance-tuning-part-ii-java.html)
+ [Garbage Collectors](https://donnachaforde.blogspot.com/2015/11/jvm-performance-tuning-part-iii-garbage.html)
+ [Heap Configuration & JIT](https://donnachaforde.blogspot.com/2016/03/jvm-performance-tuning-part-iv-heap.html)
+ [JVM Diagnostics](https://donnachaforde.blogspot.com/2017/02/jvm-performance-tuning-part-v-jvm.html)



### Background
These blogs came about after having to dig into JVM Tuning of the Hotspot VM on a development project I was working, several years ago now. We were still shipping to 32-bit OS and had reached the limits of addressable memory on that architecture. My job was to ensure we could squeeze in our memory requirements under the max threshold - and - improve performance. 

I ended up digging much deeper than originally expected. I pretty much read everything that had been published online by Sun and later, Oracle, on JVM Performance Tuning, several interesting articles and a few books. The blogs are my attempt to capture and explain the details before time moved on and I'd naturally start to forget. (JVM Performance Tuning is not my full-time job.)



> **Note** The links above redirect to pages on Blogger.com. The files in the repo itself are Word docs.
