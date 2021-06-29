## Thinking of cmake

### Exemple 3

**workspace**  : ~/tmp

> ~tmp **$** ls
>
> build CMakeLists.txt src

> ~/tmp **$** gedit CMakeLists.txt

```cmake
cmake_minimum_required(VERSION 3.1)

project(FUN)

add_subdirectory(src bin)

```

> ~/tmp **$** ls ./src
>
> CMakeLists.txt main.cpp

> ~/tmp **$** gedit ./src/CMakeLists.txt

```cmake
cmake_minimum_required(VERSION 3.1)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build)

include_directories(BEFORE /home/lihan/lianxi/lib)
link_directories(/home/lihan/lianxi)

add_executable(main main.cpp)

target_link_libraries(main libfun.so)

```

> ~/tmp **$** gedit ./src/main.cpp

```c++
#include<iostream>
#include "fun.h"

int main()
{
    fun();
    return 0;
}

```

#### Thinking

> target_link_libraries(main libfun.so)==
>
> target_link_libraries(main fun)

if you want to link static library,`target_link_libraries(main libfun.a)

`