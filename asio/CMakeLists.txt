########################################################################
#
# Global Config
#
# we use folders to manage target
set_property(GLOBAL PROPERTY USE_FOLDERS On)
########################################################################
#
# Project
#
project(asio CXX)
cmake_minimum_required(VERSION 2.6)
set (CMAKE_BACKWARDS_COMPATIBILITY 2.6)

# on windows, only support vc11
if (WIN32 AND NOT MSVC11)
  message(FATAL_ERROR "Cann't run in this VC platform toolset, "
                      "this project only support vc11")
endif()

# compiler and linker config
include("${CMAKE_CURRENT_SOURCE_DIR}/cmake/internal.cmake")
set_default_compiler_linker_config_asio()  # Defined in internal.cmake.

include_directories("${CMAKE_CURRENT_SOURCE_DIR}/include")
include_directories("D:/dummy-boost-include")

add_definitions(-DASIO_STANDALONE)
add_definitions(-DASIO_HAS_STD_TYPE_TRAITS -DASIO_HAS_STD_ARRAY -DASIO_HAS_STD_SHARED_PTR -DASIO_HAS_STD_FUNCTION)
add_definitions(-DASIO_HAS_STD_ATOMIC -D ASIO_HAS_STD_CHRONO -D ASIO_HAS_STD_CHRONO_MONOTONIC_CLOCK -D ASIO_HAS_STD_ADDRESSOF)
add_definitions(-DASIO_HAS_MOVE -DASIO_HAS_CSTDINT)

add_definitions(-DASIO_SEPARATE_COMPILATION -DASIO_ENABLE_CANCELIO)
########################################################################
#
# Library
#
file(GLOB scripts RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
    "cmake/internal.cmake")

source_group(scripts FILES ${scripts})

#file(GLOB detail RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
#    "include/asio/detail/*.hpp")
#source_group(detail FILES ${detail})

#file(GLOB impl RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
#    "include/asio/impl/*.hpp")
#source_group(impl FILES ${impl})

file(GLOB_RECURSE headers "*.hpp")
set(sources
    ${scripts}
    #${detail}
    #${impl}
    "src/asio.cpp"
    "include/asio.hpp"
    #"include/asio/*.hpp"
    ${headers}
    )

add_library(asio STATIC ${sources})
########################################################################
#
# Example
#
add_executable(chat_client "src/examples/chat/chat_client.cpp")
add_executable(chat_server "src/examples/chat/chat_server.cpp")
target_link_libraries(chat_client asio)
target_link_libraries(chat_server asio)
