---
title: Android studio 2.3 Cmake Jni 环境搭建
date: 2017-03-31 19:18:19
tags: [Android]
---

## 使用体验
    c++的支持很优雅！

# 环境需求
1、Android studio 2.3

2、NDK 版本：14.1.38

3、Cmake(SDK tools 列表里下载)

<!--more-->

## gradle配置

```
android {
    ......
    defaultConfig {
        ......
        externalNativeBuild {
            cmake {
                //以下为C开发方式
                //cppFlags ""
                
                //以下为C++开发方式，具体参数可以百度下
                cppFlags "-std=c++11", "-frtti", "-fexceptions"
            }

            ndk {
                //生成的平台支持列表
                abiFilters 'armeabi-v7a'
            }
        }
    }
    ......
    externalNativeBuild {
        cmake {
            //Cmake 配置文件
            path "CMakeLists.txt"
        }
    }

}
```

## CMakeLists示例说明
```
//版本相关
cmake_minimum_required(VERSION 3.6)


# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds it for you.
# Gradle automatically packages shared libraries with your APK.

//配置c++文件路径和头文件路径
set(CMAKE_VERBOSE_MAKEFILE on)
set(libs "${CMAKE_SOURCE_DIR}/src/main/jniLibs")
include_directories(${CMAKE_SOURCE_DIR}/src/main/cpp/include)

#--------------------------------------------------- import ---------------------------------------------------#
//根据项目自己的Jni依赖需求,需要引用第三方so库的时候，使用add_library
add_library(libopencv_java3 SHARED IMPORTED )
set_target_properties(libopencv_java3 PROPERTIES
    IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_java3.so")

//这个在gradle里配置了，这里应该是不用写的
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11 -fexceptions -frtti")

//自己的so库lib名，以及依赖的c++文件
add_library( # Sets the name of the library.
             scanner

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             # Associated headers in the same location as their source
             # file are automatically included.
             src/main/cpp/libmanager.cpp
             src/main/cpp/scanner.cpp)

find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log)

//库连接，scanner依赖了后面的静态库
target_link_libraries(scanner android log
    libopencv_java3
    ${log-lib}
    )
```