cmake_minimum_required(VERSION 3.15)
project(rcsServer)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_MACOSX_RPATH 1)

find_package(Boost REQUIRED COMPONENTS system)

if(NOT COPPELIASIM_INCLUDE_DIR)
    if(DEFINED ENV{COPPELIASIM_ROOT_DIR})
        set(COPPELIASIM_INCLUDE_DIR $ENV{COPPELIASIM_ROOT_DIR}/programming/include)
    else()
        set(COPPELIASIM_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/../include)
    endif()
endif()
list(APPEND CMAKE_MODULE_PATH ${COPPELIASIM_INCLUDE_DIR}/cmake)
find_package(CoppeliaSim 4.5.1.7 REQUIRED)

coppeliasim_add_executable(rcsServer
    SOURCES
    sourceCode/rcsServer.cpp
    ${COPPELIASIM_INCLUDE_DIR}/simLib/socketInConnection.cpp
)
target_include_directories(rcsServer PRIVATE ${COPPELIASIM_INCLUDE_DIR})
target_link_libraries(rcsServer Boost::system)
if(UNIX AND NOT APPLE)
    target_link_libraries(rcsServer dl)
endif()
