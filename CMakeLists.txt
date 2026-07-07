cmake_minimum_required(VERSION 3.20)
project(FlowKit LANGUAGES CXX)

# Require strict C++20 standard
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)


# -----------------------------------------------------------------------------
# Define FlowKit as a static library.
add_library(FlowKit STATIC
    src/flowkit.cpp
)

# Tell consumers where to find header files (.hpp)
target_include_directories(FlowKit PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

# Enable Code Coverage instrumentation exclusively for GCC/Clang in Debug mode
if(CMAKE_CXX_COMPILER_ID MATCHES "GNU" OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    if(CMAKE_BUILD_TYPE STREQUAL "Debug")
        message(STATUS "Enabling code coverage instrumentation")
        target_compile_options(FlowKit PRIVATE --coverage -O0 -g)
        target_link_options(FlowKit PRIVATE --coverage)
    endif()
endif()


# -----------------------------------------------------------------------------
include(FetchContent)
FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/refs/tags/v1.14.0.tar.gz
)
# Force GoogleTest to link to the same runtime library layout on Windows/MSVC
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)


# -----------------------------------------------------------------------------
enable_testing()

# Create a single test binary containing all component tests
add_executable(FlowKitTests
    tests/unit/graph_tests.cpp
    tests/unit/executor_tests.cpp
    tests/unit/task_tests.cpp
)

# Link test binary against library AND GoogleTest
target_link_libraries(FlowKitTests PRIVATE 
    FlowKit 
    gtest_main  # automatically provides a main() entrypoint function for tests
)

# Inject coverage flags to the test target as well during Debug compilation
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    target_compile_options(FlowKitTests PRIVATE --coverage -O0 -g)
    target_link_options(FlowKitTests PRIVATE --coverage)
endif()

# Automated test discovery via CTest framework
include(GoogleTest)
gtest_discover_tests(FlowKitTests)