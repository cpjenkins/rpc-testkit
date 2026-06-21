cmake_minimum_required(VERSION 3.30)

# RPC-TestKit v0.1.0
project(rpc-testkit
    VERSION 0.1.0
    LANGUAGES CXX
)

# Options
option(RPC_TESTKIT_DEVELOPER_MODE
    "Enable developer mode (LSP Support)" ON
)
option(RPC_TESTKIT_STRICT_WARNINGS
    "Treat warnings as compiler errors (-Werror)" ON
)
option(
    RPC_TESTKIT_WITH_SANITIZERS
    "Enable address and undefined behaviour sanitizers" OFF
)
option(
    RPC_TESTKIT_WITH_TEST_SUITE "Enable test-suite build" ON
)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_SCAN_FOR_MODULES OFF)

if(RPC_TESTKIT_DEVELOPER_MODE)
    message(STATUS "OPTION :: rpc test-kit :: developer mode is ON")

    set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
    if(NOT CMAKE_BUILD_TYPE)
        set(CMAKE_BUILD_TYPE Debug CACHE STRING "select build type (Debug, Release)" FORCE)
    endif()
endif()

# Define compiler warning options
add_library(compiler_warnings INTERFACE)
target_compile_options(compiler_warnings INTERFACE
    -Wall
    -Wextra
    -Wpedantic
    -Wshadow
    -Wnon-virtual-dtor
    -Wold-style-cast
    -Wcast-align
    -Wunusued
    -Woverloaded-virtual
    -Wconversion
    -Wsign-conversion
)

# Enable sanitizers (ASan and UBSan)
if(RPC_TESTKIT_WITH_SANITIZERS)
    message(STATUS "OPTION :: rpc test-kit :: address and undefined behavior sanitization is ON")
    target_compile_options(compiler_warnings INTERFACE
        -fsanitize=address,undefined
        -fno-omit-frame-pointer
    )
    target_link_options(compiler_warnings INTERFACE
        -fsanitize=address,undefined
    )
endif()

# Enable test suite
# if(RPC_TESTKIT_WITH_TEST_SUITE)
#     enable_testing()
# endif()
add_subdirectory(third_party)
add_subdirectory(source)
add_subdirectory(tests)