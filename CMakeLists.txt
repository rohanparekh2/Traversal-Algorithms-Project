cmake_minimum_required(VERSION 3.1)

# Set Policies
if(POLICY CMP0048)
  cmake_policy(SET CMP0048 NEW)
endif()

if(POLICY CMP0023)
  cmake_policy(SET CMP0023 NEW)
endif()

if(POLICY CMP0079)
  cmake_policy(SET CMP0079 NEW)
endif()

# Specify the C++ standard (CMake3+)
add_compile_options(-std=c++11)

# Add header files
file(GLOB INCLUDES "include/*.h")

# Add src files
file(GLOB SOURCES "src/*.cpp")

# Add testing sources
file(GLOB TEST_SOURCES "tests/*.cpp")

# Add library interface for our files
add_library(FinalProject ${SOURCES} ${INCLUDES})

# Link
target_include_directories(FinalProject PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

# Add the executable
add_executable(main main.cpp)
target_link_libraries(main FinalProject)

target_include_directories(main PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})


# Add Catch2
Include(FetchContent)

FetchContent_Declare(
    Catch2
    GIT_REPOSITORY https://github.com/catchorg/Catch2.git
    GIT_TAG v2.13.1
)

FetchContent_MakeAvailable(Catch2)
add_executable(test ${SOURCES} ${TEST_SOURCES})
target_link_libraries(test FinalProject Catch2::Catch2)