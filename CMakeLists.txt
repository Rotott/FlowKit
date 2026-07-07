cmake_minimum_required(VERSION 3.20)

project(FlowKit LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_executable(FlowKit
    "src/flowkit.cpp"
 "tests/unit/graph_tests.cpp" "tests/unit/executor_tests.cpp" "tests/unit/task_tests.cpp")

# Enable Code Coverage for GCC/Clang in Debug mode 
if(CMAKE_CXX_COMPILER_ID MATCHES "GNU" OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    if(CMAKE_BUILD_TYPE STREQUAL "Debug")
        message(STATUS "Enabling code coverage instrumentation")

        # Using target_compile_options and target_link_options ensures 
        # these options explicitly stick to the FlowKit binary and any testing targets
        target_compile_options(FlowKit PRIVATE --coverage -O0 -g)
        target_link_options(FlowKit PRIVATE --coverage)
        
        # If you add a test binary later like: add_executable(FlowKitTests test.cpp)
        # You will want to make sure it also gets these flags:
        # target_compile_options(FlowKitTests PRIVATE --coverage -O0 -g)
        # target_link_options(FlowKitTests PRIVATE --coverage)
    endif()
endif()




#OLD BELOW

# Enable Code Coverage for GCC/Clang in Debug mode 
#if(CMAKE_CXX_COMPILER_ID MATCHES "GNU" OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
 #   if(CMAKE_BUILD_TYPE STREQUAL "Debug")
  #      message(STATUS "Enabling code coverage instrumentation")
   #     add_compile_options(--coverage -O0 -g)
    #    add_link_options(--coverage)
    #endif()
#endif()