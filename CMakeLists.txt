cmake_minimum_required(VERSION 3.24)
include(CMakePrintHelpers)

set(CMAKE_CXX_STANDARD 17)

set(PATCH_VERSION "2" CACHE INTERNAL "Patch version")
set(PROJECT_VERSION 0.0.${PATCH_VERSION})
set(Boost_NO_WARN_NEW_VERSIONS 1)

project(Task_1 VERSION  ${PROJECT_VERSION})
configure_file(version.h.in version.h)

set(Boost_DBUG ON)
cmake_print_variables(Boost_INCLUDE_DIR)

find_package(Boost COMPONENTS unit_test_framework REQUIRED)

add_executable(Task_1 main.cpp)
add_executable(test_version test_version.cpp)

set_target_properties(Task_1 test_version PROPERTIES CXX_STANDARD 17 CXX_STANDARD_REQUIRED ON)

target_include_directories(Task_1 PRIVATE "${CMAKE_BINARY_DIR}")

set_target_properties(test_version PROPERTIES COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR})

target_compile_options(Task_1 PRIVATE
        -Wall -Wextra -pedantic -Werror
        )

install(TARGETS Task_1 RUNTIME DESTINATION bin)
set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_DEBIAN_PACKAGE_MAINTAINER "Vishnenko Maxim")

include(CPack)

enable_testing()
add_test(test_version test_version)