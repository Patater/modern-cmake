cmake_minimum_required(VERSION 3.1)

project(RootExampleDict LANGUAGES CXX)

find_package(ROOT CONFIG REQUIRED)
include("${ROOT_DIR}/modules/RootNewMacros.cmake")

message(STATUS "Found ROOT: ${ROOT_VERSION} at ${ROOT_DIR}")

string(REPLACE " " ";" ROOT_CXX_FLAG_LIST "${ROOT_CXX_FLAGS}")

set_target_properties(ROOT::Core PROPERTIES
    INTERFACE_INCLUDE_DIRECTORIES "${ROOT_INCLUDE_DIRS}"
    INTERFACE_COMPILE_OPTIONS "${ROOT_CXX_FLAG_LIST}"
    )

include_directories(ROOT_BUG)
root_generate_dictionary(G__MyExample MyExample.h LINKDEF SimpleLinkDef.h)

add_library(MyExample SHARED MyExample.cxx MyExample.h G__MyExample.cxx)
target_link_libraries(MyExample PUBLIC ROOT::Core)

#enable_testing()
#add_test(NAME MyExample COMMAND )
