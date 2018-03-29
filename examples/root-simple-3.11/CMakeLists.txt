cmake_minimum_required(VERSION 3.11)

project(RootSimpleExample311 LANGUAGES CXX)

find_package(ROOT CONFIG REQUIRED)
message(STATUS "Found ROOT: ${ROOT_VERSION} at ${ROOT_DIR}")

string(REPLACE " " ";" ROOT_CXX_FLAG_LIST "${ROOT_CXX_FLAGS}")

## [modern_fix]
target_include_directories(ROOT::Core INTERFACE "${ROOT_INCLUDE_DIRS}")
target_compile_options(ROOT::Core INTERFACE "${ROOT_CXX_FLAG_LIST}")
## [modern_fix]

add_executable(RootSimpleExample311 SimpleExample.cxx)
target_link_libraries(RootSimpleExample311 PUBLIC ROOT::Physics)

enable_testing()
add_test(NAME RootSimpleExample311 COMMAND RootSimpleExample311)
