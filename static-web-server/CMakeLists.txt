cmake_minimum_required(VERSION 3.7.2)
project(highload-cpp-webserver)

# CMAKE FLAGS
set(CMAKE_CPP_FLAGS "-pedantic -fprofile-arcs -ftest-coverage -g")

set(CMAKE_CXX_STANDARD 20)


# LIB
set(HIGHLOAD_SERVER_LIB_PATH ${PROJECT_SOURCE_DIR})

file(GLOB LIB_SOURCES ${PROJECT_SOURCE_DIR}/src/*.cpp)


add_library(highload_server_lib STATIC
    ${LIB_SOURCES}
)

target_include_directories(highload_server_lib
    PUBLIC
        ${PROJECT_SOURCE_DIR}/include
)



set_target_properties(highload_server_lib PROPERTIES LINKER_LANGUAGE C)
#
# LIB


# CLI
# 
add_executable(highload_server main.cpp)

target_link_libraries( highload_server 
    PRIVATE
        highload_server_lib
)

# CLI
