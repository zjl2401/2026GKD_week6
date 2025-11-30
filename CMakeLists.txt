cmake_minimum_required(VERSION 3.15)
project(week6 
    VERSION 1.0.0
    DESCRIPTION "Week6 Trie Data Structure Project"
    LANGUAGES CXX
)

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# 设置默认构建类型为 Release（如果没有指定）
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
endif()

# 编译选项
if(MSVC)
    # MSVC 编译器选项
    add_compile_options(/W4 /permissive-)
    # 启用 C++20 std::format 支持（需要 Visual Studio 2019 16.10+）
    add_compile_definitions(_HAS_CXX20)
else()
    # GCC/Clang 编译器选项
    add_compile_options(-Wall -Wextra -Wpedantic)
    if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
        # GCC 需要特定版本支持 std::format
        if(CMAKE_CXX_COMPILER_VERSION VERSION_LESS "11.0")
            message(WARNING "GCC version < 11.0 may not fully support std::format")
        endif()
    endif()
endif()

# 根据构建类型设置优化选项
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    add_compile_definitions(DEBUG)
    if(MSVC)
        add_compile_options(/Od /Zi)
    else()
        add_compile_options(-g -O0)
    endif()
else()
    if(MSVC)
        add_compile_options(/O2)
    else()
        add_compile_options(-O3 -DNDEBUG)
    endif()
endif()

# 包含头文件目录
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

# 添加可执行文件
add_executable(week6 
    trie.cpp
    test.cpp
)

# 设置输出目录
set_target_properties(week6 PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin
)

# 打印配置信息
message(STATUS "Project: ${PROJECT_NAME}")
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
message(STATUS "C++ Standard: ${CMAKE_CXX_STANDARD}")
message(STATUS "Compiler: ${CMAKE_CXX_COMPILER_ID} ${CMAKE_CXX_COMPILER_VERSION}")
