#==============================
# Requirements
#==============================

cmake_minimum_required(VERSION 3.10)

#==============================
# Project Settings
#==============================

set(TARGET_NAME "TemplateExecutable")
set(TARGET_NAMESPACE "Gaia")
set(TARGET_VERSION 0.9.0)

if (NOT PROJECT_DECLARED)
    project("${TARGET_NAMESPACE} ${TARGET_NAME}" LANGUAGES CXX VERSION ${TARGET_VERSION})
    set(PROJECT_DECLARED)
endif()

#==============================
# Command Lines
#==============================

set(CMAKE_CXX_STANDARD 17)

#==============================
# Source
#==============================

# C++ Source Files
FILE(GLOB_RECURSE TARGET_SOURCE *.cpp)
# C++ Header Files
FILE(GLOB_RECURSE TARGET_HEADER *.hpp)

#==============================
# Compile Targets
#==============================

# To lower the running cost, this module will be compiled as a static library.
add_executable(${TARGET_NAME} ${TARGET_SOURCE} ${TARGET_HEADER} ${TARGET_CUDA_SOURCE} ${TARGET_CUDA_HEADER})

# Enable 'DEBUG' Macro in Debug Mode
if(CMAKE_BUILD_TYPE STREQUAL Debug)
    target_compile_definitions(${TARGET_NAME} PRIVATE -DDEBUG)
endif()

#==============================
# Dependencies
#==============================

target_include_directories(${TARGET_NAME} PUBLIC "../")

# In Linux, 'Threads' need to explicitly linked.
if(CMAKE_SYSTEM_NAME MATCHES "Linux")
    find_package(Threads)
    target_link_libraries(${TARGET_NAME} PUBLIC ${CMAKE_THREAD_LIBS_INIT})
endif()