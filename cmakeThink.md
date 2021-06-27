## Thinking of cmake

### Example  1

workspace: ~/lianxi

~/lianxi **$**  ls

bin build CMakeLists.txt src

~/lianxi **$** gedit CMakeLists.txt

```cmake

cmake_minimum_required(VERSION 3.1)

project(HELLO)

add_subdirectory(src ${PROJECT_SOURCE_DIR}/bin)

```

~/lianxi/src **$** ls

CMakeLists.txt  hello.cpp  hello.h sorry.cpp

~/lianxi/src **$**  gedit  CMakeLists.txt

```cmake

cmake_minimum_required(VERSION 3.1)

project(HELLO)

set(CMAKE_CXX_STANDARD 11)

set(CMAKE_CXX_STANDARD_REQUIRED True)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR})

set(HELLO_SRC hello.cpp sorry.cpp)

add_executable(hello ${HELLO_SRC})

```

~/lianxi/src **$**  gedit hello.cpp


```c++

#include <iostream>

#include "hello.h"

using namespace std;

int main()
{
    cout<<"hello,world!"<<endl;
    sorry();
    return 0;
}

```

~/lianxi/src **$**  gedit hello.c

```c++

void sorry();

```


~/lianxi/src **$**  gedit sorry.cpp

```c++

#include<iostream>

using namespace std;

void sorry()
{
    cout<<"sorry,I'm not world."<<endl;
}

```

#### Thinking

`add_subdirectory(src ${PROJECT_SOURCE_DIR}/bin)`

>src: Indicating where is source directory
>
>${PROJECT_SOURCE_DIR}/bin: Indicating where are output_files when compiling.

`CMAKE_SOURCE_DIR vs PROJECT_SOURCE_DIR`

>First, we must understand not every CMakeLists.txt has a project_name

>Second, project_name which in ~/lianxi/CMakeLists.txt or ~/lianxi/src/CMakeLists.txt
>maybe diferrent.

>Third, when PROJECT_SOURCE_DIR in ~/lianxi/CMakeLists.txt, that means ~/lianxi
>
>when PROJECT_SOURCE_DIR in ~/lianxi/src/CMakeLists.txt which a project_name in, that means ~/lianxi/src
>
>when PROJECT_SOURCE_DIR in ~/lianxi/src/CMakeLists.txt which a project_name doesn't in, that means ~/lianxi

>Forth, CMAKE_SOURCE_DIR means ~/lianxi
>
>when CMAKE_CURRENT_SOURCE_DIR in~/lianxi/src/CMakeLists.txt, that means ~/lianxi/src

`set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOUTCE_DIR})`

>The variable of CMAKE_RUNTIME_OUTPUT_DIRECTORY indicates executable file's location,
>if not set, executable file's location will be decided by command `add_subdirectory`



