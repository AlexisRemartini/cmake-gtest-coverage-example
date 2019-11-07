project(pop)
cmake_minimum_required(VERSION 2.8)
option(CodeCoverage "CodeCoverage" ON)
#Was OFF. WHy do I have to set it by hand?
set(CMAKE_CXX_FLAGS "-std=c++11 ${CMAKE_CXX_FLAGS}")
#-Wno-deprecated-register
set(CMAKE_CXX_FLAGS_DEBUG "-std=c++11 -O0 -g -fprofile-arcs -ftest-coverage ${CMAKE_CXX_FLAGS_DEBUG}")
#-Wno-deprecated-register
set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake/modules/)

add_subdirectory(tests)
add_subdirectory(vendor)

add_executable(pop main.cpp)
target_link_libraries(pop LINK_PUBLIC vendor)

#need to call with -DCMAKE_BUILD_TYPE=Debug
if (CMAKE_BUILD_TYPE MATCHES Debug)
if (CodeCoverage MATCHES ON)
message("Oh, it's on!")
include(CodeCoverage)
setup_target_for_coverage(${PROJECT_NAME}_coverage poptests coverage)
#setup_target_for_coverage_cobertura(${PROJECT_NAME}_cobertura poptests coverage --gtest_output=xml:coverage.junit.xml)
#CMake Error at cmake/modules/CodeCoverage.cmake:144 (MESSAGE):
#  gcovr not found! Aborting...
#Call Stack (most recent call first):
#  CMakeLists.txt:22 (setup_target_for_coverage_cobertura)

endif()
endif()
