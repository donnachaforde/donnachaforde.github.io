
[home/](../../)[blogs+posts/](../)[jvm-performance-tuning](./)

# JVM Performance Tuning

<img src="./rcs/duke-thumbs-up.png" width="300" height="300" />

A five-part blog on **Java Virtual Machine Tuning** covering:

+ [JVM Concepts](./java-performance-tuning-part-i-jvm-concepts) 
+ [The Java Memory Model](./java-performance-tuning-part-ii-the-java-memory-model)
+ [Garbage Collectors](./java-performance-tuning-part-iii-garbage-collectors)
+ [Heap Configuration & JIT](./java-performance-tuning-part-iv-heap-configuration-and-jit)
+ [JVM Diagnostics](./java-performance-tuning-part-v-jvm-diagnostics) 



### Background
This blog series came into being after I was forced to do extensive research into JVM Tuning and wanted to 'capture' the knowledge gleamed from that effort. At the time, I was working on a development project that shipped to a 32-bit OS and we'd reached the limits of addressable memory. My job was to ensure we could squeeze our application onto that hardware - and - improve performance. 

I ended up digging much deeper than originally expected and pretty much read everything on JVM Performance Tuning that had been published online by Sun, and later by Oracle. I share what I learned here in the hope it provides a shortcut for other engineers facing into JVM Tuning. 



> **Note**
> This blog series was originally published on [blogger.com](https://donnachaforde.blogspot.com) between 2015-2017. The articles were subsequently migrated to [GitHub Pages](https://donnachaforde.github.io/blogs+posts/jvm-performance-tuning/) in 2023 where minor edits and corrections were applied. Any further updates will only be applied here. 
