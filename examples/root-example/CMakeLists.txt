cmake_minimum_required(VERSION 3.1)

project(RootExample LANGUAGES CXX)

find_package(ROOT CONFIG REQUIRED)

message(STATUS "Found ROOT: ${ROOT_VERSION} at ${ROOT_DIR}")

string(REPLACE " " ";" ROOT_CXX_FLAG_LIST "${ROOT_CXX_FLAGS}")

set_target_properties(ROOT::Core PROPERTIES
    INTERFACE_INCLUDE_DIRECTORIES "${ROOT_INCLUDE_DIRS}"
    INTERFACE_COMPILE_OPTIONS "${ROOT_CXX_FLAG_LIST}"
    )

add_executable(MyExample MyExample.cxx)
target_link_libraries(MyExample PUBLIC ROOT::Physics)

enable_testing()
add_test(NAME MyExample COMMAND MyExample)
