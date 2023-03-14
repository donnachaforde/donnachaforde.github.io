[github.com](https://github.com/donnachaforde/donnachaforde.github.io) | [github.io](https://donnachaforde.github.io)

# Visual Studio Build-Debugging Tips
When you hit compiler and linker errors in Visual Studio, it can help if you peel the output back to basics and pay close attention to what you’re telling the compiler and linker – and what it’s saying back to you. 

First, it’s important you understand that the C++ compiler (and the linker) that Visual Studio uses isn’t necessarily  part of the IDE as such but is a separate executable, invoked by the IDE when you ask it to compile and build. Back in the day, Microsoft were releasing their C/C++ compiler before anyone dreamt up the idea of an 'integrated development environment'. Technically, Visual Studio performs a CreateProcess() on the `cl.exe` process passing along your compiler option defaults and then redirects the stdout from the process via a pipe back to the Visual Studio output window. In more recent versions, it formats this output into more structured report but, personally, having started out with early versions of VS, I like to examine the raw output itself. I find it easier to search. 

## Examine the output
When I really need to scrutinize the compile output, I copy this output to a separate text editor and, in order to use UNIX/Linux tools like `grep`, save the file off somewhere convenient. However, this last step is unnecessary as Visual Studio writes this output to its own build directories according to platform (e.g. x64) and configuration (i.e. debug/release) into a text file called \<project-name>.log. Note that these are the intermediate files for the project, not the solution output itself. The solution output (e.g. exe, dll) is written to the solution output directory, whose location depends on the physical layout of the project but, by default, will be in subdirectory under the solution file itself. 


## Massage the message
One of the downsides of introducing templates into C++ is that compiler errors can be so verbose as to be near impossible for a human to read. One advantage of copying build output to a separate editor, is to replace expanded template definitions with human-readable shorthand. For example, the humble `std::string` expands to `std::basic_string<char,std::char_traits<char>,std::allocator<char>>` and can make a function/method call seem quite verbose. I’ve found myself making these sort of substitutions with STL constructs (like `list` and `queue`) just to make the error more readable. Once your build output starts to resemble readable code again, you're half-way to understanding the error at hand.

Let's take an example of a linker error I recently encoutered while working with gRPC. On initial presentation, the following error-message is somewhat opaque. 

````
libprotobuf-lite.lib(libprotobuf-lite.dll) : error LNK2005: "public: void __cdecl google::protobuf::internal::ArenaStringPtr::Set(struct google::protobuf::internal::ArenaStringPtr::EmptyDefault,class std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> > &&,class google::protobuf::Arena *)" (?Set@ArenaStringPtr@internal@protobuf@google@@QEAAXUEmptyDefault@1234@$$QEAV?$basic_string@DU?$char_traits@D@std@@V?$allocator@D@2@@std@@PEAVArena@34@@Z) already defined in libprotobuf.lib(arenastring.obj)
````
But, we can start by breaking out the message text and the mangled signature from what is actually code. 
````
libprotobuf-lite.lib(libprotobuf-lite.dll) : error LNK2005: 

"public: void __cdecl google::protobuf::internal::ArenaStringPtr::Set(struct google::protobuf::internal::ArenaStringPtr::EmptyDefault,class std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> > &&,class google::protobuf::Arena *)" 

(?Set@ArenaStringPtr@internal@protobuf@google@@QEAAXUEmptyDefault@1234@$$QEAV?$basic_string@DU?$char_traits@D@std@@V?$allocator@D@2@@std@@PEAVArena@34@@Z) 

already defined in libprotobuf.lib(arenastring.obj)
````

This is the mangled representation of the function. I've found that whereas these used to be fairly simple when using C, because of namespace, classes, overloading, etc. these are more unweildy. 
````
(?Set@ArenaStringPtr@internal@protobuf@google@@QEAAXUEmptyDefault@1234@$$QEAV?$basic_string@DU?$char_traits@D@std@@V?$allocator@D@2@@std@@PEAVArena@34@@Z) 
````

This is the actually C++ code detailing the method in question. 

````
"public: void __cdecl google::protobuf::internal::ArenaStringPtr::Set(struct google::protobuf::internal::ArenaStringPtr::EmptyDefault,class std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> > &&,class google::protobuf::Arena *)" 
````

Now, we can substitute the full representation for the string class. 
````
"public: void __cdecl google::protobuf::internal::ArenaStringPtr::Set(struct google::protobuf::internal::ArenaStringPtr::EmptyDefault, string&&, class google::protobuf::Arena *)" 
````

It's still a little unweildy so let's get rid of namespace and class scoping. 
````
"public: void google::protobuf::internal::ArenaStringPtr::Set(struct google::protobuf::internal::ArenaStringPtr::EmptyDefault, string&&, class google::protobuf::Arena *)" 
````

Finally, something that is human-readable. We're looking to resolve a method that take 3 arguments (i.e. a struct, a string and a class). 

````
"public: void internal::ArenaStringPtr::Set(struct internal::ArenaStringPtr::EmptyDefault, string&&, class Arena*)" 
````

Now at least , we know what we're dealing with. 


## Reveal your compiler and linker Instructions
By default, Visual Studio supresses the detailed compiler and linker execution banner. It uses the /nologo switch respectively on the compiler and linker to achieve this. I like to see what I’m going and occasionally, need to check what include paths or linker paths I’ve actually specified in the project properties. 

To enable the startup banner, go to Project Properties, select C/C++ --> General and change ‘Suppress Startup Banner’ from Yes (/nologo) to No. 
You can do likewise under Linker --> General to see the linker output. As a matter of course, I do this for debug builds because, as an engineer, I want to see what's going on.


## Simplify the message 
When it comes to linker errors, you can get quite a large number of instances of the error for what might actually be the same underlying issue. As with compiler errors, each individual linker error has its own unique code (e.g. [LNK2005](https://docs.microsoft.com/en-us/cpp/error-messages/tool-errors/linker-tools-error-lnk2005). 
You can use this unique number with `grep` to either filter them out or gauge how many you're getting. 
While we are talking about Windows development here, I find it useful to leverage UNIX/Linux utils to work at the command-line and I always install a Bash shell (e.g. Git Bash). 

The following will show the linker errors (where ‘client’ is the name of the project):

```
$ cat client.log | grep "error LNK" 
```



And, this will show the number of these errors:

```
$ cat client.log | grep -c "error LNK"
```



If you have recurring error (like `LNK2038: mismatch detected or error LNK2005: - already defined`), then you can filter these out to get an idea of what you’re dealing with. 
e.g. 
```
$ cat client.log | grep "error LNK" | grep -v LNK2038 | grep -v LNK2005

client.exe : fatal error LNK1169: one or more multiply defined symbols found
```

Filtering the output in this way shows us that we really only have 2-3 types of issues to resolve. 
A quick read up on [LNK1169](https://docs.microsoft.com/en-us/cpp/error-messages/tool-errors/linker-tools-error-lnk1169) explains it normally follows instances of [LNK2005](https://docs.microsoft.com/en-us/cpp/error-messages/tool-errors/linker-tools-error-lnk2005).
Ultimately, what might initially appeared as scary with hundreds of errors just boils down to two. 

## Extended logs exists
The visual studio build actually writes this information to it’s own logs in a dedicated directory under the project output called \<project-name>.tlog (same location as the temporary project files described above). The same compiler command revealed by not suppressing the start-up banner is logged to cat `CL.command.1.tlog`.

Unfortunately, the corresponding linker file – i.e. `link.command.1.tlog` – doesn’t contain any text. Both files seem to start with a binary signature so they don’t work with tools like grep. (I understand they are ‘Unicode’ files.)
