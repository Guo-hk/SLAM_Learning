[大佬上课](https://www.bilibili.com/video/BV1fy4y1b7TC?p=16)



## SLAM中常用库的CMakeList.txt写法（如何添加各种依赖库）

#### 1.添加Eigen头文件

```cmake
find_package(Eigen3 REQUIRED) #查找Eigen3

include_directories(${EIGEN3_INCLUDE_DIR}) #添加头文件

target_link_libraries(${PROJECT_NAME} ${EIGEN3_LIBS}) #链接到工程中
```

#### 2.添加Pangolin依赖

​	功能主要就是做三维的可视化显示。

```cmake
find_package(Pangolin REQUIRED) #查找Pangolin
 
include_directories(${Pangolin_INCLUDE_DIRS}) #添加头文件

target_link_libraries(${PROJECT_NAME} ${Pangolin_LIBRARIES}) #链接到工程中
```

#### 3.添加Sophus依赖

​	Sophus实际上是Eigen库的扩展模块，Eigen中虽然有几何模块，但是没有提供李代数的支持，所以Sophus算是一个比较好的李代数库。

```cmake
find_package(Sophus REQUIRED) #查找Souphs

include_directories(${Sophus_INCLUDE_DIRS}) #添加头文件

target_link_libraries(${PROJECT_NAME} ${Sophus_LIBRARIES}) #链接到工程中
```

#### 4.添加OpenCV依赖

​	OpenCV经常会出现版本不兼容的问题，如果同时安装了OpenCV2和OpenCV3两个版本，所以在CMakeLists.txt可以使用条件语句查找。

​	添加OpenCV要注意一个问题，大小写！很重要！大小写！

```cmake
find_package(OpenCV 3.0 QUIET) #查找OpenCV
if (NOT OpenCV_FOUND)
    find_package(OpenCV 2.4.3 QUIET)
    if (NOT OpenCV_FOUND)
        message(FATAL_ERROR "OpenCV > 2.4.3 not found.")
    endif ()
endif ()

include_directories(${OpenCV_INCLUDE_DIRS}) #添加头文件

target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBS}) #链接到工程中
```

#### 5.添加PCL依赖

​	点云库

```cmake
find_package(PCL REQUIRED COMPONENT common io) #查找PCL
 
include_directories(${PCL_INCLUDE_DIRS}) #添加头文件
 
add_definitions(${PCL_DEFINITIONS}) #比较特殊，暂时还不知道什么意思
 
target_link_libraries(${PROJECT_NAME} ${PCL_LIBRARIES}) #链接到工程中
```

#### 6.添加Ceres依赖

​	Ceres是Google出品的一个优化库，安装编译都在LZ之前写过一个SLAM安装大全里都有。因为Ceres不是常用的库，所以需要添加一个cmake_modules。

```cmake
#这行代码就是添加查找Ceres的一个文件
list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake_modules)
 
find_package(Ceres REQUIRED) #查找Ceres
 
include_directories(${CERES_INCLUDE_DIRS}) #添加头文件
 
target_link_libraries(${PROJECT_NAME} ${CERES_LIBRARIES}) #链接到工程中
```

#### 7.添加G2O的依赖

​	第一个，要在cmake_module中假如findG2O的文件。第二个，注意大小写问题，还有数字0和字母0，这个还是要注意的。

```cmake
#这行代码就是添加查找G2O的一个文件
list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake_modules)
 
find_package(G2O REQUIRED)
 
include_directories(${G20_INCLUDE_DIRS})
 
target_link_libraries(${PROJECT_NAME} g2o_core g2o_stuff)
```

### 将自己写的.cc文件和头文件链接到工程中

1.向工程添加头文件的搜索路径

```cmake
#一般将头文件放到工程根目录的include文件夹中
include_directories(
        ${PROJECT_SOURCE_DIR}
        ${PROJECT_SOURCE_DIR}/include
        ）
```

2.生成库文件，将.cc文件都生成为共享库文件，这样就不用在src文件夹下再写一个CMakeLists.txt文件了，但是在链接.cc文件的时候要加上路径

```cmake
add_library(${PROJECT_NAME} SHARED
		src/System.cc
        src/Tracking.cc
        src/LocalMapping.cc
        ...
		)
```







## CMake教程

### **Cmake中一些预定义变量**

> * cmake中一些预定义变量:
>
> `PROJECT_SOURCE_DIR` 工程的根目录
> `PROJECT_BINARY_DIR` 运行cmake命令的目录,通常是`${PROJECT_SOURCE_DIR}/build`
> `CMAKE_INCLUDE_PATH` 环境变量,非cmake变量
> `CMAKE_LIBRARY_PATH` 环境变量
> `CMAKE_CURRENT_SOURCE_DIR` 当前处理的CMakeLists.txt所在的路径
> `CMAKE_CURRENT_BINARY_DIR target`编译目录
> 使用`ADD_SURDIRECTORY(src bin)`可以更改此变量的值
> `SET(EXECUTABLE_OUTPUT_PATH <新路径>)`并不会对此变量有影响,只是改变了最终目标文件的存储路径
> `CMAKE_CURRENT_LIST_FILE` 输出调用这个变量的CMakeLists.txt的完整路径
> `CMAKE_CURRENT_LIST_LINE` 输出这个变量所在的行
> `CMAKE_MODULE_PATH` 定义自己的cmake模块所在的路径
> `SET(CMAKE_MODULE_PATH  ${PROJECT_SOURCE_DIR}/cmake)`,然后可以用INCLUDE命令来调用自己的模块
> `EXECUTABLE_OUTPUT_PATH` 重新定义目标二进制可执行文件的存放位置
> `LIBRARY_OUTPUT_PATH` 重新定义目标链接库文件的存放位置
> `PROJECT_NAME` 返回通过PROJECT指令定义的项目名称
> `CMAKE_ALLOW_LOOSE_LOOP_CONSTRUCTS` 用来控制IF ELSE语句的书写方式
>
> * 系统信息
>
> `CMAKE_MAJOR_VERSION` cmake主版本号,如2.8.6中的2
> `CMAKE_MINOR_VERSION` cmake次版本号,如2.8.6中的8
> `CMAKE_PATCH_VERSION` cmake补丁等级,如2.8.6中的6
> `CMAKE_SYSTEM` 系统名称,例如Linux-2.6.22
> `CAMKE_SYSTEM_NAME` 不包含版本的系统名,如Linux
> `CMAKE_SYSTEM_VERSION` 系统版本,如2.6.22
> `CMAKE_SYSTEM_PROCESSOR` 处理器名称,如i686
> UNIX 在所有的类UNIX平台为TRUE,包括OS X和cygwin
> WIN32 在所有的win32平台为TRUE,包括cygwin
>
> * 开关选项 
>
> `BUILD_SHARED_LIBS` 控制默认的库编译方式。如果未进行设置,使用ADD_LIBRARY时又没有指定库类型,默认编译生成的库都是静态库 
>
> `CMAKE_BUILD_TYPE` 设置模式是Debug还是Release模式

```cmake
SET(CMAKE_BUILD_TYPE "Release")

SET(CMAKE_BUILD_TYPE "Debug”)
```

> `CMAKE_C_FLAGS` 设置C编译选项
> `CMAKE_CXX_FLAGS` 设置C++编译选项

 **编译选项具体说明：**

 在CMakeLists.txt中可能会看到这样的命令设置C或者C++的编译选项：

```cmake
SET(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS} -O0 -Wall -g -ggdb")
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -O3 -Wall")
```

> 后面跟随的-O3 -Wall的是什么意思？这个参数是gcc或者g++进行编译时设置的参数；
>
> gcc中常用的编译选项有：
>
> -c 只编译并生成目标文件：将汇编代码编译成目标文件，即二进制代码。-c 可以直接把 C/C++ 代码编译成机器代码，注意此时并没有链接生成可执行文件这样的步骤，因此，对于链接中的错误是无法发现的。
>
> -g 生成调试信息。GNU 调试器可利用该信息。
>
> -S 只是编译不汇编，生成汇编代码
>
> -shared 生成共享目标文件。通常用在建立共享库时。 
> -W 开启所有 gcc 能提供的警告。 
> -w 不生成任何警告信息。 
> -Wall 生成所有警告信息。
>
>  -O0 -O1 -O2 -O3 四级优化选项：
> -O0 不进行优化处理： 不做任何优化，这是默认的编译选项 
> -O 或 -O1 优化生成代码。 优化会消耗少多的编译时间，它主要对代码的分支，常量以及表达式等进行优化
> -O2 进一步优化。会尝试更多的寄存器级的优化以及指令级的优化，它会在编译期间占用更多的内存和编译时间 
>
> -Os 相对语-O2.5。相当于-O2.5。是使用了所有-O2的优化选项，但又不缩减代码尺寸的方法。
> -O3 比 -O2 更进一步优化，包括 inline 函数。在O2的基础上进行更多的优化 





### CMake常用命令

#### **1）project 命令**

* 命令语法：project(<projectname> [languageName1 languageName2 … ] )
* 命令简述：用于指定项目的名称
* 使用范例：project(Main)

#### **2）cmake_minimum_required命令**

* 命令语法：`cmake_minimum_required(VERSION major[.minor[.patch[.tweak]]][FATAL_ERROR])`

* 命令简述：用于指定需要的 CMake 的最低版本

* 使用范例：`cmake_minimum_required(VERSION 2.8)`

#### **3）aux_source_directory命令**

* 命令语法：`aux_source_directory(<dir> <variable>)`

* 命令简述：用于将 dir 目录下的所有源文件的名字保存在变量 variable 中

* 使用范例：`aux_source_directory(. DIR_SRCS)`，将当前目录下的所有源文件加入到DIR_SRCS，如果多个源文件，则DIR_SRCS是一个list。

#### **4）add_executable 命令**

* 命令语法：`add_executable(<name> [WIN32] [MACOSX_BUNDLE][EXCLUDE_FROM_ALL] source1 source2 … sourceN)`

* 命令简述：用于指定从一组源文件 source1 source2 … sourceN 编译出一个可执行文件且命名为 name

* 使用范例：`add_executable(Main ${undefinedDIR_SRCS})`

#### **5）add_library 命令**

* 命令语法：`add_library([STATIC | SHARED | MODULE] [EXCLUDE_FROM_ALL] source1source2 … sourceN)`

* 命令简述：用于指定从一组源文件 source1 source2 … sourceN 编译出一个库文件且命名为 name

* 使用范例：`add_library(Lib ${DIR_SRCS})`，和add_executable相比，这里是成的库，而add_executable是生成可执行文件。因此，这里的STATIC、SHARED分别表示在生成的是静态库还是动态库。

#### **6）add_dependencies 命令**

* 命令语法：`add_dependencies(target-name depend-target1 depend-target2 …)`

* 命令简述：用于指定某个目标（可执行文件或者库文件）依赖于其他的目标。这里的目标必须是 add_executable、add_library、add_custom_target 命令创建的目标

#### 7）add_subdirectory 命令

* 命令语法：`add_subdirectory(source_dir [binary_dir] [EXCLUDE_FROM_ALL])`

* 命令简述：用于添加一个需要进行构建的子目录，意味着该目录下也有个CMakeLists.txt 文件

* 使用范例：`add_subdirectory(Lib)`


#### 8）target_link_libraries与link_libraries命令

* 两者关系: link_libraries用在add_executable之前，target_link_libraries用在add_executable之后; 

  link_libraries用来链接静态库，target_link_libraries用来链接导入库，即按照header file + .lib + .dll方式隐式调用动态库的.lib库

* 命令语法：`target_link_libraries(<target> [item1 [item2 […]]][[debug|optimized|general] ] …)`

* 命令简述：用于指定 target 需要链接 item1 item2 …。这里 target 必须已经被创建，链接的 item 可以是已经存在的 target（依赖关系会自动添加）

* 使用范例：`target_link_libraries(Main Lib)；link_libraries(/home/myproject/libs)`

* 解释：为Main项目（可以为执行文件，也可以为库）添加依赖库Lib。Lib可以为多个。比如：动态库所在目录为/home/myproject/libs，那么在add_executable之前使用link_libraries(/home/myproject/libs)指明动态库位置，在add_executable之后，只需给出动态链接库的库名字就行。target_link_libraries(Main -lcurl)

#### 9）set 命令

* 命令语法：`set(<variable> <value> [[CACHE <type><docstring> [FORCE]] | PARENT_SCOPE])`

* 命令简述：用于设定变量 variable 的值为 value。如果指定了 CACHE 变量将被放入 Cache（缓存）中。

* 使用范例1：`set(ProjectName Main)`

* 使用范例2：`set(var a;b;c)` <=> `set(var a b c)`  #定义变量var并赋值为a;b;c这样一个string list


#### 10）unset 命令

* 命令语法：`unset(<variable> [CACHE])`

* 命令简述：用于移除变量 variable。如果指定了 CACHE 变量将被从 Cache 中移除。

* 使用范例：`unset(VAR CACHE)`


#### 11）message 命令

* 命令语法：`message([STATUS|WARNING|AUTHOR_WARNING|FATAL_ERROR|SEND_ERROR] “message todisplay”…)`

* 命令简述：用于输出信息，STATUS表示正常提示，WARNING就是警告 提示，FATAL_ERROR重大错误提示...

* 使用范例：`message(STATUS “Hello World”)`，STATUS可以不添加。


#### 12）include_directories 命令

* 命令语法：`include_directories([AFTER|BEFORE] [SYSTEM] dir1 dir2 …)`

* 命令简述：用于设定目录，这些设定的目录将被编译器用来查找 include 文件

* 使用范例：`include_directories(${PROJECT_SOURCE_DIR}/lib)`，添加在工程目录下的lib子文件夹目录


#### 13）find_path 命令

* 命令语法：`find_path(<VAR> name1 [path1 path2 …])`

* 命令简述：用于查找包含文件 name1 的路径，如果找到则将路径保存在 VAR 中（此路径为一个绝对路径），如果没有找到则结果为 <VAR>-NOTFOUND。默认的情况下，VAR 会被保存在 Cache 中，这时候我们需要清除 VAR 才可以进行下一次查询（使用 unset 命令）。

* 使用范例：


```cmake
find_path(LUA_INCLUDE_PATH lua.h${LUA_INCLUDE_FIND_PATH})

if(NOT LUA_INCLUDE_PATH)

   message(SEND_ERROR "Header file lua.h not found")

endif()
```



#### 14）find_library 命令

* 命令语法：`find_library(<VAR> name1 [path1 path2 …])`

* 命令简述：用于查找库文件 name1 的路径，如果找到则将路径保存在 VAR 中（此路径为一个绝对路径），如果没有找到则结果为 <VAR>-NOTFOUND。一个类似的命令 link_directories 已经不太建议使用了


#### 15）add_definitions 命令

* 命令语法：`add_definitions(-DFOO -DBAR …)`

* 命令简述：用于添加编译器命令行标志（选项），通常的情况下我们使用其来添加预处理器定义

* 使用范例：`add_definitions(-D_UNICODE -DUNICODE)`,  使用细节参考[这里](https://blog.csdn.net/fb_941219/article/details/107376017)


#### **16）find_package：**

把整个依赖包的头文件包含路径、库路径、库名字、版本号等情况都获取到，比如opencv的相关信息：

```cmake
cmake_minimum_required (VERSION 2.6)
project (ImageBlend)
add_definitions(-std=c++11)
find_package(OpenCV 3.2.0 REQUIRED )
# find_package(OpenCV 4.1.0 REQUIRED )
include_directories( ${OpenCV_INCLUDE_DIRS} )
add_executable(ImageBlend main2.cpp)
target_link_libraries( ImageBlend ${OpenCV_LIBS} )
```

`find_package(OpenCV 3.2.0 REQUIRED )`，其中的3.2.0是制定版本。如果系统中版本过多，不加此版本号，就按照相应的搜索规则进行，那么就可能面临版本号不对应出现的问题



### **make的常用命令**

在cmake 编译了相关文件后，还需要make一下，相当于编译文件。make还有一些其他命令如下：

`make all`：编译程序、库、文档等（等同于make）

`make install`：安装已经编译好的程序。复制文件树中到文件到指定的位置

`make unistall`：卸载已经安装的程序。

`make clean`：删除由make命令产生的文件

`make distclean`：删除由./configure产生的文件







