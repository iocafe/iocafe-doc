Windows build notes 
====================
So far only used build system for Windows has been Visual Studio 2019. There are many other tools
the build for Windows: It is possible to use CMAKE files also on Windows and build with MinGW/Qtcreator,
other Visual Studio version, etc, but these are likely to require minor tinkering with build setup and possibly with code.  

Build folder organization and .sln files
*****************************************
Each library and example project directory contains subdirectory named build/vs2019. The <projectname>.sln solution
file and the <projectname>.vcxproj is project file. All the other files and folders within vs2019 folder are temporary
intermediate build and user setting files. For libraries, the solution file contains only reference library’s project
file. For example projects, the solution file, in addition to example’s project file, lists all library projects
needed to build the example. To build, open the solution file .sln in Visual Studio 2019, select if you want
32/64 bit compilation and debug/release build. 

Microsoft Visual Studio 2019 pitfalls (27.6.2019)
**************************************************
As previous versions, VS 2019 has nice debugging and other tools. 22.5.2020 note: Some of these may be fixed in new
releases of Visual Studio 2019, at least these have not annoyed me lately.

* By default it comes with “automatic code formatting” enabled. This will totally mess up your code readability, 
  and is unsustainable for projects with lot of code, long code life time, multiple environments or multiple 
  developers. This must be disabled before using, instructions from Google.
* Rebuild when using multiple copies of Visual Studio simultaneously to debug multiple executables at same time
  doesn’t work. If these are based on same libraries, something is always locked in other copy of visual Studio
  by other and prevents the build. It looks like someone has reported problem that door hinges sometimes give
  a sound, so Microsoft solved the problem by nailing whole door shut. Workaround to debug two executables is
  to use 64 bit build for other and 32 bit build for the other. 
* Debugging doesn’t always work if network connection is not up to Microsoft “specs” (what ever those are).
  In practice this means that all works when at home, but when traveling I often need to turn off WiFi 
  networking. Otherwise debugging doesn’t start, but Visual studio just locks up.


200202, updated 23.5.2020/pekka

