cmake_minimum_required(VERSION 3.4...3.18)
project(cmake_example)

# Make a separate library from mymath
# - could deploy mymath as library (ship headers and .a)
# - create executable for unit testing
# - create separate executable for an app or etc.
add_library(mymath STATIC src/mymath.cpp)
set_target_properties(mymath PROPERTIES POSITION_INDEPENDENT_CODE ON)

add_subdirectory(pybind11)
pybind11_add_module(cmake_example src/binder.cpp)
target_link_libraries(cmake_example PRIVATE mymath)

# EXAMPLE_VERSION_INFO is defined by setup.py and passed into the C++ code as a
# define (VERSION_INFO) here.
target_compile_definitions(cmake_example
    PRIVATE VERSION_INFO=${EXAMPLE_VERSION_INFO})
