## Think of cmake

### Example 2

**workspace:** ~lianxi

>~lianxi **$** ls
>
> bin build CMakeLists.txt COPYRIGHT doc lib README runhello.sh src

>~lianxi **$** gedit CMakeLists.txt

```cmake
cmake_minimum_required(VERSION 3.1)

project(HELLO)

#Two or more "add_subdirectory" command in one file,must sure output_binary_directories are direffent and unique.
add_subdirectory(src ${CMAKE_SOURCE_DIR}/bin)
add_subdirectory(lib ${CMAKE_SOURCE_DIR}/bin/lib)

install(FILES COPYRIGHT README DESTINATION share/doc/hello)

install(PROGRAMS runhello.sh DESTINATION bin)

install(DIRECTORY doc/ DESTINATION share/doc/hello)

```

> ~/lianxi **$** ls ./src
>
>CMakeLists.txt hello.cpp hello.h sorry.cpp

> ~lianxi **$** gedit ./src/CMakeLists.txt
 
```cmake
cmake_minimum_required(VERSION 3.1)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build)

set(HELLO_SRC hello.cpp sorry.cpp)

add_executable(hello ${HELLO_SRC})

install(TARGETS hello RUNTIME DESTINATION bin)

```

> ~/lianxi **$** ls ./lib
>
> CMakeLists.txt fun.cpp fun.h

> ~/lianxi **$** gedit CMakeLists.txt

```cmake
cmake_minimum_required(VERSION 3.1)

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib)

set(LIBFUN_SRC fun.cpp)


#Create shared library and static library.
add_library(fun SHARED ${LIBFUN_SRC})
add_library(fun_static STATIC ${LIBFUN_SRC})

#Let static library and shared library have same_name.  
set_target_properties(fun_static PROPERTIES OUTPUT_NAME "fun")

#Let shared library and static library which have same_name exist together.
set_target_properties(fun PROPERTIES CLEAN_DIRECT_OUTPUT 1)
set_target_properties(fun_static PROPERTIES CLEAN_DIRECT_OUTPUT 1)

#VERSION:shared library version, SOVERSION:API version.
set_target_properties(fun PROPERTIES VERSION 1.2 SOVERSION 1)

install(TARGETS hello hello_static 
           LIBRARY DESTINATION lib     
           ARCHIVE DESTINATION lib)
           
install(FILES fun.h DESTINATION include)

```

> ~/lianxi **$** gedit ./lib/fun.h

```c++
#ifndef FUN_H
#define FUN_H

#include <iostream>

void fun();



#endif

```

> ~/lianxi **$** gedit ./lib/fun.cpp

```c++
#include "fun.h"

void fun()
{
    printf("I'm fun, thank you, and you?\n");
}

```

#### Thinking

> How "install" ?
    
    After set in related CMakeLists.txt, cd in 'build' subdirectory, Using command "cmake -DCMAKE_INSTALL_PREFIX=/usr .." to perform, then "make","make install".

> Why "install" ?

    After installed your project, you can easy to use this project in your new project or direct use executable file which produced from your installed_project in the future.

> Why "library" ?
    
    Using library make your project looks like more simplify. And you can use one library in many differnt project, save a lot of time. 

> shared library VS static library

    Linking time: 

        {static library}:It happens as the last step of the compilation process. After the program is placed in the memory

        {shared library}:Shared libraries are added during linking process when executable file and libraries are added to the memory.

    Means:

        {static library}:Performed by linkers

        {shared library}:Performed by operating System

    Size:

        {static library}:Static libraries are much bigger in size, because external programs are built in the executable file.

        {shared library}:Dynamic libraries are much smaller, because there is only one copy of dynamic library that is kept in memory.

    External file changes:

        {static library}:Executable file will have to be recompiled if any changes were applied to external files.

        {shared library}:In shared libraries, no need to recompile the executable.

    Time:

        {static library}:Takes longer to execute, because loading into the memory happens every time while executing.

        {shared library}:It is faster because shared library code is already in the memory.

    compatibility:

        {static library}:Never has compatibility issue, since all code is in one executable module.

        {shared library}:Programs are dependent on having a compatible library. Dependent program will not work if library gets removed from the system .

    Special applacation:
        {static library}:use static libraries when needing to ensure that the binary does not have many external dependencies that may be difficult to meet, such as specific versions of the C++ standard library or specific versions of the Boost C++ library.

        {shared library}:hared libraries can be loaded into an application at run-time, which is the general mechanism for implementing binary plug-in systems.

    FUNNY EXEMPLE:
        A static library is like a bookstore, and a shared library is like... a library. With the former, you get your own copy of the book/function to take home; with the latter you and everyone else go to the library to use the same book/function. So anyone who wants to use the (shared) library needs to know where it is, because you have to "go get" the book/function. With a static library, the book/function is yours to own, and you keep it within your home/program, and once you have it you don't care where or when you got it.
    
