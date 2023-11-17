[home/](https://donnachaforde.github.io)[blogs+posts/](https://donnachaforde.github.io/blogs+posts/)[jvm-performance-tuning](https://donnachaforde.github.io/blogs+posts/jvm-performance-tuning/)


# JVM Performance Tuning



A five-part blog on `Java Virtual Machine Tuning` covering:

+ [JVM Concepts](./Java%20Performance%20Tuning%20-%20Part%20I%20-%20JVM%20Concepts.md) - Java memory management & garbage collection.
+ [The Java Memory Model](./Java%20Performance%20Tuning%20-%20Part%20II%20-%20The%20Java%20Memory%20Model.md) - JVM memory layout & nomenclature.
+ [Garbage Collectors](./Java%20Performance%20Tuning%20-%20Part%20III%20-%20Garbage%20Collectors.md) - Configuring JVM garbage collectors.
+ [Heap Configuration & JIT](./Java%20Performance%20Tuning%20-%20Part%20IV%20-%20Heap%20Configuration%20&%20JIT.md) - Configuring the JVM Heap & the _Just in Time_ compiler.
+ [JVM Diagnostics](./Java%20Performance%20Tuning%20-%20Part%20V%20-%20JVM%20Diagnostics.md) - Pulling everything together and performing analysis.





### Background
This blog series came into being after I was forced to do extensive research into JVM Tuning and wanted to 'capture' the knowledge gleamed from that effort. At the time, I was working on a development project that shipped to a 32-bit OS and we'd reached the limits of addressable memory. My job was to ensure we could squeeze our application onto that hardware - and - improve performance. 

I ended up digging much deeper than originally expected and pretty much read everything on JVM Performance Tuning that had been published online by Sun, and later by Oracle. I share what I learned here in the hope it provides a shortcut for other engineers facing into JVM Tuning. 



> [!NOTE]
> This blog series was originally published on [blogger.com](https://donnachaforde.blogspot.com) between 2015-2017. The articles were subsequently migrated to [GitHub Pages](https://donnachaforde.github.io/blogs+posts/jvm-performance-tuning/) in 2023 where edits and corrections have been applied and will continue to be applied. 
