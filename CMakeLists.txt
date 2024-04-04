# Request the minimum version of CMake, in case of lower version throws error.
cmake_minimum_required(VERSION 3.25 FATAL_ERROR)

#Define the project
project(
    "FirstProyectInCMake"                                           # Name
    VERSION 1.0.0                                                   # Version
    DESCRIPTION "CMake C Project Template for Operating Systems II" # Description
    LANGUAGES C                                                  # Language
)

#Enable testings and add the tests
include(CTest)

#Define the C standard, std17
set(CMAKE_C_STANDARD 17)

#Define the header files
#Include the 'include' directory, where the header files are located
include_directories(include)

#To automatic dowload external deps, we need use 'FetchContent'
#Me permite descargar dependencias de internet
include(FetchContent)
set(UNITY_GIT_URL "https://github.com/ThrowTheSwitch/Unity.git")
set(FETCHCONTENT_QUIET OFF)

FetchContent_Declare(
    Unity
    GIT_REPOSITORY ${UNITY_GIT_URL}
    SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/external/UNITY_BUILD
    UPDATE_COMMAND ""
)

FetchContent_MakeAvailable(Unity)

#Define the source files
#Include the 'src' directory, where the source files are located
file(GLOB_RECURSE SOURCES "src/*.c")
#ventaja: no tengo que escribir cada archivo que quiero incluir, solo la carpeta
#Desventaja: si tengo archivos que no estan en la carpeta, no se incluiran

#Define the executable
add_executable(${PROJECT_NAME} ${SOURCES})

#Compile the libraries
#CMake aislados los va a incluir como proyectos independientes
add_subdirectory(lib/libdyn)
add_subdirectory(lib/libsta)

#include headers in lib
#incluye los ejecutables
target_include_directories(${PROJECT_NAME} PUBLIC lib/libdyn/include)
target_include_directories(${PROJECT_NAME} PUBLIC lib/libsta/include)

#Link the libraries
target_link_libraries(${PROJECT_NAME} StatiCLib DynamiCLib)

#Add subdirectories of tests
if(RUN_TESTS EQUAL 1 OR RUN_COVERAGE EQUAL 1)
    add_subdirectory(tests)
endif()