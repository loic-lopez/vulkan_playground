cmake_minimum_required(VERSION 3.21)
project(vulkan_playground)

set(CMAKE_CXX_STANDARD 20)

add_definitions(-DUSE32bit=false)
add_definitions(-DCMAKE_GENERATOR_PLATFORM=x64)

if (MSVC)
    add_compile_options(/W4 /WX)
elseif (CMAKE_COMPILER_IS_GNUCXX)
    # lots of warnings and all warnings as errors
    add_compile_options(-Wall -Wextra -pedantic -Werror)
endif ()

# Download automatically, you can also just copy the conan.cmake file
if(NOT EXISTS "${CMAKE_BINARY_DIR}/conan.cmake")
    message(STATUS "Downloading conan.cmake from https://github.com/conan-io/cmake-conan")
    file(DOWNLOAD "https://raw.githubusercontent.com/conan-io/cmake-conan/0.17.0/conan.cmake"
            "${CMAKE_BINARY_DIR}/conan.cmake"
            TLS_VERIFY ON)
endif()

include(${CMAKE_BINARY_DIR}/conan.cmake)
conan_cmake_run(
        CONANFILE
        conanfile.txt
        BASIC_SETUP
        BUILD missing
        SETTINGS compiler.cppstd=20
)

include(${CMAKE_BINARY_DIR}/conan_paths.cmake)

include_directories(include)

find_package(glfw3 REQUIRED)
find_package(Vulkan REQUIRED)

add_executable(vulkan_playground src/main.cpp)

conan_target_link_libraries(vulkan_playground)
