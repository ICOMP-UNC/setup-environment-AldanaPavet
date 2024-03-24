# Request the minimum version of CMake, in case of lower version throws error.
cmake_minimum_required(VERSION 3.25 FATAL_ERROR)

#Define the project
project(
    "FirstProyectInCMake"                                           # Name
    VERSION 1.0.0                                                   # Version
    DESCRIPTION "CMake C Project Template for Operating Systems II" # Description
    LANGUAGES C                                                  # Language
)

#Define the C standard, std17
set(CMAKE_C_STANDARD 17)

#Define the header files
#Include the 'include' directory, where the header files are located
include_directories(include)

#Define the source files
#Include the 'src' directory, where the source files are located
file(GLOB SOURCES "src/*.c")
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