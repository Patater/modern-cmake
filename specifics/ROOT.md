# ROOT

ROOT is a C++ Toolkit for High Energy Physics. It is huge. There are really a lot of ways to use it in CMake, though many/most of the examples you'll find are probably wrong. Here's my recommendation.


## Finding ROOT

ROOT supports config file discovery, so you can just do:

```cmake
find_package(ROOT CONFIG)
```

to attempt to find ROOT. If you don't have your paths set up, you can pass `-DROOT_DIR=$ROOTSYS/cmake` to find ROOT. (But, really, you should source `thisroot.sh`)

## The wrong way

ROOT provides a utility to set up a ROOT project, which you can activate using `include(${ROOT_USE_FILE})`. This will automatically make ugly global variables for you. It will save you a little time setting up, and will waste massive amounts of time later when you try to do anything more complicated.  Don't use it.

## The right way (Targets)

ROOT does not correctly set up it's imported targets. To fix this error, you'll need something like:

```cmake
find_package(ROOT CONFIG REQUIRED)

string(REPLACE " " ";" ROOT_CXX_FLAG_LIST "${ROOT_CXX_FLAGS}")

set_target_properties(ROOT::Core PROPERTIES
    INTERFACE_INCLUDE_DIRECTORIES "${ROOT_INCLUDE_DIRS}"
    INTERFACE_COMPILE_OPTIONS "${ROOT_CXX_FLAG_LIST}"
    )
```

In CMake 3.11, you can replace that last function call with:

```cmake
target_include_directories(ROOT::Core IMPORTED INTERFACE "${ROOT_INCLUDE_DIRS}")
target_compile_options(ROOT::Core IMPORTED INTERFACE "${ROOT_CXX_FLAG_LIST}")
```

All the ROOT targets will require `ROOT::Core`, so this will be enough regardless of which ROOT targets you need.

To link, just pick the libraries you want to use:

```cmake
add_executable(MyExample MyExample.cxx)
target_link_libraries(MyExample PUBLIC ROOT::Physics)
```

## Dictionary generation

Dictionary generation is ROOT's way of working around the missing reflection feature in C++. It allows ROOT to learn the details of your class so it can save it, show methods in the Cling interpreter, etc. You'll need three things in your source code to make it work for classes:

* Your class definition should end with `ClassDef(MyClassName, 1)`
* Your class implementation should have `ClassImp(MyClassName)` in it
* You should have a file with a name that ends with `LinkDef.h`

The `LinkDef.h` file follows a specific formula and tells ROOT what parts to generate dictionaries for.

To generate, you should include the following in your CMakeLists:

```cmake
include("${ROOT_DIR}/modules/RootNewMacros.cmake")
include_directories(ROOT_BUG)
```
The second line is due to a bug in the NewMacros file that causes dictionary generation to fail if there is not at least one global include directory or a `inc` folder. Here I'm including a non-existent directory just to make it work. There is no `ROOT_BUG` directory.

To generate a file:

```cmake
root_generate_dictionary(G__MyExample MyExample.h LINKDEF SimpleLinkDef.h)
```

Then include G__MyExample.cxx in your sources when you make the library.

## Example: Minimal

#### examples/root-example/CMakeLists.txt
[include](../examples/root-example/CMakeLists.txt)
