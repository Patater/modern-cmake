
## [main]
cmake_minimum_required(VERSION 3.1)

project(RootDictExample LANGUAGES CXX)

find_package(ROOT CONFIG REQUIRED)
include("${ROOT_DIR}/modules/RootNewMacros.cmake")

message(STATUS "Found ROOT: ${ROOT_VERSION} at ${ROOT_DIR}")

string(REPLACE " " ";" ROOT_CXX_FLAG_LIST "${ROOT_CXX_FLAGS}")

set_target_properties(ROOT::Core PROPERTIES
    INTERFACE_INCLUDE_DIRECTORIES "${ROOT_INCLUDE_DIRS}"
    INTERFACE_COMPILE_OPTIONS "${ROOT_CXX_FLAG_LIST}"
    )

include_directories(ROOT_BUG)
root_generate_dictionary(G__DictExample DictExample.h LINKDEF DictLinkDef.h)

add_library(DictExample SHARED DictExample.cxx DictExample.h G__DictExample.cxx)
target_link_libraries(DictExample PUBLIC ROOT::Core)

## [main]

enable_testing()
add_test(NAME RootDictExample
    WORKING_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}"
    COMMAND "${ROOT_root_CMD}" -b -l -q "${CMAKE_CURRENT_SOURCE_DIR}/CheckLoad.C")
