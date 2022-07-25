# Visual Studio Build-Debugging Tips
When you hit compiler and linker errors in Visual Studio, it can help if you peel the output back to basics and pay close attention to what you’re telling the compiler and linker – and what it’s saying back to you. 

First, it’s important you understand that the C++ compiler (and the linker) that Visual Studio uses isn’t really part of the IDE as such but is a separate executable, invoked by the IDE when you ask it to compile and build. Technically, it does a CreateProcess on the ‘cl’ process passing along your settings (i.e. compiler switches) and redirects the stdout from that cl.exe process via a pipe back to the Visual Studio output window. In more recent versions, it formats this output into more structured report but, personally, having started out with early versions of VS, I like to examine the raw output itself. I find it easier to search. 

## Examine the output
When I really need to scrutinize the compile output, I copy this output to a separate text editor and in order to use UNIX/Linux tools like ‘grep’, save the file off somewhere convenient. However, this last step is unnecessary as Visual Studio writes this output to its own build directories according to platform (e.g. x64) and configuration (i.e. debug/release) into a text file called <project-name>.log. 
Note: These are the intermediate files for the project, not the solution. The solution output (e.g. exe, dll) is written to the solution output, whose location depends on the physical layout of the project but, by default, will be in subdirectory under the solution file itself. 
One of the downsides of introduces templates into C++ is that compiler errors can be so verbose as to be near impossible for a human to read. One advantage of copying build output to a separate editor, is to replace expanded template definitions with human-readable shorthand. 

For example, the humble `std::string` expands to `std::basic_string<char,std::char_traits<char>,std::allocator<char>>` and can make a function/method call seem quite verbose. I’ve found myself making these sort of substitutions with STL constructs just to make the error more readable. 

## Reveal your compiler and linker Instructions
By default, Visual Studio supresses the detailed compiler and linker execution banner. It uses the /nologo switch respectively on the compiler and linker to achieve this. I like to see what I’m going and occasionally, need to check what include paths or linker paths I’ve actually specified in the project properties. 

To enable the startup banner, go to Project Properties, select C/C++ --> General and change ‘Suppress Startup Banner’ from Yes (/nologo) to No. 
You can do likewise under Linker --> General to see the linker output. 

## Extended logs
The visual studio build actually writes this information to it’s own logs in a dedicated directory under the project output called <project-name>.tlog (same location as the temporary project files described above).
The same compiler command revealed by not suppressing the start-up banner is logged to cat CL.command.1.tlog. 
Unfortunately, the corresponding linker file – i.e. link.command.1.tlog – doesn’t contain any text. Both files seem to start with a binary signature so they don’t work with tools like grep. (I understand they are ‘Unicode’ files.)

## Simplify your errors
When it comes to linker errors, you can get quite a large number of error instances for what might actually be the same underlying issue. As with compiler errors, each individual linker error has its own unique code (e.g. https://docs.microsoft.com/en-us/cpp/error-messages/tool-errors/linker-tools-error-lnk2005 )
You can use this unique number with grep to either filter them out or gauge how many your getting. 
While we are talking about Windows development here, I find it useful to leverage UNIX/Linux utils to work at the command-line. 

```
$ cat client.log | grep "error LNK" 
```

will show the linker errors (where ‘client’ is the name of the project). And, 

```
$ cat client.log | grep -c "error LNK"
```
will show the number of these errors.


If you have recurring error (like LNK2038: mismatch detected or error LNK2005: - already defined), then you can filter these out to get an idea of what you’re dealing with. 
e.g. 
```
$ cat client.log | grep "error LNK" | grep -v LNK2038 | grep -v LNK2005

client.exe : fatal error LNK1169: one or more multiply defined symbols found
```

showing us that we really only have 2-3 types of issues to resolve. 
A quick read up on https://docs.microsoft.com/en-us/cpp/error-messages/tool-errors/linker-tools-error-lnk1169 explains it normally follows instances of LNK2005.
Ultimately, what might initially appeared as scary with hundreds of errors just boils down to two. 
