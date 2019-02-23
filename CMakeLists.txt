project (rlib)

cmake_minimum_required(VERSION 2.8)

## use C++11 features
add_definitions(-std=c++11)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_CXX_COMPILER /usr/bin/g++)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g")

## path to ralloc lib, which you need to build manually
set(RALLOC_LIB "${PROJECT_SOURCE_DIR}/ralloc/libssmalloc.a")

## include paths
include_directories(ralloc)

file(GLOB SOURCES "*.hpp")

add_library(rlib STATIC ${SOURCES})
set_target_properties(rlib PROPERTIES LINKER_LANGUAGE CXX)
target_link_libraries(rlib -lpthread ibverbs ${RALLOC_LIB})
install(TARGETS rlib DESTINATION lib)

add_executable(server "example/server.cpp")
add_executable(client "example/client.cpp")

target_link_libraries(server rlib)
target_link_libraries(client rlib)

