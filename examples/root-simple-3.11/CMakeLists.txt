
## [main]
cmake_minimum_required(VERSION 3.11)

project(RootSimpleExample311 LANGUAGES CXX)

find_package(ROOT CONFIG REQUIRED)
message(STATUS "Found ROOT: ${ROOT_VERSION} at ${ROOT_DIR}")

separate_arguments(ROOT_CXX_FLAGS)
separate_arguments(ROOT_DEFINITIONS)
message(STATUS "ROOT_DEFINITIONS ${ROOT_DEFINITIONS}")

# This is required on if there is more than one flag (like on macOS)
# and this also fixes a bug in the linker flags
string(REPLACE "-L " "-L" ROOT_EXE_LINKER_FLAGS "${ROOT_EXE_LINKER_FLAGS}")
separate_arguments(ROOT_EXE_LINKER_FLAGS)

## [modern_fix]
target_include_directories(ROOT::Core INTERFACE "${ROOT_INCLUDE_DIRS}")

add_library(ROOT::Flags_CXX IMPORTED INTERFACE)
target_compile_options(ROOT::Flags_CXX INTERFACE ${ROOT_CXX_FLAGS})
target_compile_definitions(ROOT::Flags_CXX INTERFACE ${ROOT_DEFINITIONS})
set_property(TARGET ROOT::Flags_CXX PROPERTY
    INTERFACE_LINK_LIBRARIES ${ROOT_EXE_LINKER_FLAGS})
## [modern_fix]

add_executable(RootSimpleExample311 SimpleExample.cxx)
target_link_libraries(RootSimpleExample311 PUBLIC ROOT::Physics ROOT::Flags_CXX)
## [main]

enable_testing()
add_test(NAME RootSimpleExample311 COMMAND RootSimpleExample311)
