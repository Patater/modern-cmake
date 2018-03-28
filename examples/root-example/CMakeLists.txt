
## [main]
cmake_minimum_required(VERSION 3.1)

project(RootSimpleExample LANGUAGES CXX)

## [find_package]
find_package(ROOT CONFIG REQUIRED)
## [find_package]

message(STATUS "Found ROOT: ${ROOT_VERSION} at ${ROOT_DIR}")

# ROOT targets are missing includes and flags
## [setup_properties]
# Fix for ROOT_CXX_FLAGS not actually being a CMake list
string(REPLACE " " ";" ROOT_CXX_FLAG_LIST "${ROOT_CXX_FLAGS}")

set_property(TARGET ROOT::Core PROPERTY
    INTERFACE_INCLUDE_DIRECTORIES "${ROOT_INCLUDE_DIRS}")
set_property(TARGET ROOT::Core APPEND PROPERTY
    INTERFACE_COMPILE_OPTIONS ${ROOT_CXX_FLAG_LIST})
## [setup_properties]

## [add_and_link]
add_executable(MyExample MyExample.cxx)
target_link_libraries(MyExample PUBLIC ROOT::Physics)
## [add_and_link]

## [main]

enable_testing()
add_test(NAME RootSimpleExample COMMAND MyExample)
