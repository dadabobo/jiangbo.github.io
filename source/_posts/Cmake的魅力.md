---
title: Cmake的魅力
date: 2017-02-06 14:59:26
tags: cmake
categories: cmake
---

## CMake

> 对于大工程来说，Cmake的重量级别与作用显得尤为必要。
> 要使用Cmake，也很简单，遵循几个关键的语法段即可。

>a. **想了解更多的戳这里[cmake 语法[官方]](https://cmake.org/cmake/help/v3.0/manual/cmake-language.7.html#syntax)**
>b. [更为人性的文档Cmake](https://www.jetbrains.com/help/clion/2016.3/quick-cmake-tutorial.html)  


在使用的工程里创建Cmakelists.txt，需要编译的文件按照cmake约定的语法添加目标文件。

以下为cmake**关键字段**
##  **project **  
  	  project(Target Name)  #target name定义借鉴工程意义

##  **set** 
```
	set(target var   source var ) # 将source 变量放入target 中  
	# source 变量可包含文件、路径、消息、编译工具链、编译选项等
eg1. 
	 set(CMAKE_CXX_STANDARD  11) # enable C++11 
eg2
	 set(message  " Here should add someting ")
eg3 
	 set(SELF_TEST_DIR  ${CATCH_DIR}/projects/SelfTest)	

```
								
## **add_executable**
	add_excutable(bin name  source file ) # 根据source file 生成目标可执行文件
	 
## **add_library**
```
    add_library (my_library STATIC|SHARED|MODULE ${SOURCE_FILES})
    
    to build  a library 
    - static :   bulid static library   (*.a)
    - shared:  build share library (*.so)
    - module:  build  plugin library ,  it can be load dynamicall at runningtime 
```
 
## **target_link_libraries**
```
	target_link_libraies(target   libname) #目标文件需要依赖于libname 的库
```
 
## **target_compile_options**
```
	target_compile_options( target file or bin   MODE -Wall -Wextra )
	
	MODE有以下区分，其中差异见以下，个人理解为，所取公共环境变量的依赖关系不一样。
	- private : 
	- public :
	- interface :    
```
以上差异有两种
[差异1](https://cmake.org/cmake/help/v3.1/prop_tgt/COMPILE_DEFINITIONS.html#prop_tgt:COMPILE_DEFINITIONS)  vs  [差异2](https://cmake.org/cmake/help/v3.1/prop_tgt/INTERFACE_COMPILE_DEFINITIONS.html#prop_tgt:INTERFACE_COMPILE_DEFINITIONS) 

 
##  **function**
```
	主要格式 如下
	function()
	..... #实现内部特定工作
	endfunction 
	
eg:
	function(CheckFileList LIST_VAR FOLDER)
  set(MESSAGE " should be added to the variable ${LIST_VAR}")
  set(MESSAGE "${MESSAGE} in ${CMAKE_CURRENT_LIST_FILE}\n")
  file(GLOB GLOBBED_LIST "${FOLDER}/*.cpp"
                         "${FOLDER}/*.hpp"
                         "${FOLDER}/*.h")
  list(REMOVE_ITEM GLOBBED_LIST ${${LIST_VAR}})
  foreach(EXTRA_ITEM ${GLOBBED_LIST})
    string(REPLACE "${CATCH_DIR}/" "" RELATIVE_FILE_NAME "${EXTRA_ITEM}")
    message(AUTHOR_WARNING "The file \"${RELATIVE_FILE_NAME}\"${MESSAGE}")
  endforeach()
endfunction()	
```
## **include_directories**
```
	include_directories(head file path) # 包含头文件目录 
```