
# ROOT

ROOT is a C++ Toolkit for High Energy Physics. It is huge. There are really a lot of ways to use it in CMake, though many/most of the examples you'll find are probably wrong. Here's my recommendation.

Most importantly, there are *lots of improvements* in CMake support in more recent versions of ROOT - Using 6.16+ is much, much easier! If you really must support 6.14 or earlier, see the section at the end.

## Finding ROOT

ROOT 6.10+ supports config file discovery, so you can just do:

[import:'find_package', lang:'cmake'](../../examples/root-simple/CMakeLists.txt)

to attempt to find ROOT. If you don't have your paths set up, you can pass `-DROOT_DIR=$ROOTSYS/cmake` to find ROOT. (But, really, you should source `thisroot.sh`).


## The right way (Targets)

ROOT 6.12 and earlier do not add the include directory for imported targets. ROOT 6.14+ has corrected this error, and required target properties have been getting better. This method is rapidly becoming easier to use (see the example at the end of this page for the older ROOT details).

To link, just pick the libraries you want to use:

[import:'add_and_link', lang:'cmake'](../../examples/root-simple/CMakeLists.txt)

If you'd like to see the default list, run `root-config --libs` on the command line. In Homebrew ROOT 6.18 this would be:

* `ROOT::Core`
* `ROOT::Gpad`
* `ROOT::Graf3d`
* `ROOT::Graf`
* `ROOT::Hist`
* `ROOT::Imt`
* `ROOT::MathCore`
* `ROOT::Matrix`
* `ROOT::MultiProc`
* `ROOT::Net`
* `ROOT::Physics`
* `ROOT::Postscript`
* `ROOT::RIO`
* `ROOT::ROOTDataFrame`
* `ROOT::ROOTVecOps`
* `ROOT::Rint`
* `ROOT::Thread`
* `ROOT::TreePlayer`
* `ROOT::Tree`

## The old global way

ROOT [provides a utility](https://root.cern.ch/how/integrate-root-my-project-cmake) to set up a ROOT project, which you can activate using `include("${ROOT_USE_FILE}")`. This will automatically make ugly directory level and global variables for you. It will save you a little time setting up, and will waste massive amounts of time later if you try to do anything tricky. As long as you aren't making a library, it's probably fine for simple scripts. Includes and flags are set globally, but you'll still need to link to `${ROOT_LIBRARIES}` yourself, along with possibly `ROOT_EXE_LINKER_FLAGS` (You will have to `separate_arguments` first before linking or you will get an error if there are multiple flags, like on macOS). Also, before 6.16, you have to manually fix a bug in the spacing.

Here's what it would look like:

[import:'core', lang:'cmake'](../../examples/root-usefile/CMakeLists.txt)

## Components

Find ROOT allows you to specify components. It will add anything you list to `${ROOT_LIBRARIES}`, so you might want to build your own target using that to avoid listing the components twice. This did not solve dependencies; it was an error to list `RooFit` but not `RooFitCore`. If you link to `ROOT::RooFit` instead of `${ROOT_LIBRARIES}`, then `RooFitCore` is not required.

## Dictionary generation

Dictionary generation is ROOT's way of working around the missing reflection feature in C++. It allows ROOT to learn the details of your class so it can save it, show methods in the Cling interpreter, etc. You'll need three things in your source code to make it work for classes:

* Your class definition should end with `ClassDef(MyClassName, 1)`
* Your class implementation should have `ClassImp(MyClassName)` in it
* You should have a file with a name that ends with `LinkDef.h`

The `LinkDef.h` file follows a [specific formula][linkdef-root] and tells ROOT what parts to generate dictionaries for.

To generate, you should include the following in your CMakeLists:

```cmake
include("${ROOT_DIR}/modules/RootNewMacros.cmake")

# Uncomment for ROOT versions than 6.16
# They break if nothing is in the global include list!
# include_directories(ROOT_BUG)
```

The second line is due to a bug in the NewMacros file that causes dictionary generation to fail if there is not at least one global include directory or a `inc` folder. Here I'm including a non-existent directory just to make it work. There is no `ROOT_BUG` directory.

To generate a file:

```cmake
root_generate_dictionary(G__Example Example.h LINKDEF ExampleLinkDef.h)
```

The final argument, listed after `LINKDEF`, must have a name that ends in `LinkDef.h`. This command will create three files. If you started output name with `G__`, that will be removed from the name, otherwise it will use the name given; this must match the final output library name you will soon be creating. Assuming this is `${NAME}`:

* `${NAME}.cxx`: This file should be included in your sources when you make the library.
* `lib{NAME}.rootmap` (`G__` prefix removed): The rootmap file in plain text
* `lib{NAME}_rdict.pcm` (`G__` prefix removed): A ROOT file

The final two output files must sit next to the library output. This is done by checking `CMAKE_LIBRARY_OUTPUT_DIRECTORY` (it will not pick up local target settings). If you have a libdir set but you don't have (global) install locations set, you'll also need to set `ARG_NOINSTALL` to `TRUE`. 

[linkdef-root]: https://root.cern.ch/selecting-dictionary-entries-linkdefh

---

# Using Old ROOT

If you really have to use older ROOT, you'll need something like this:

```cmake
# ROOT targets are missing includes and flags in ROOT 6.10 and 6.12
set_property(TARGET ROOT::Core PROPERTY
    INTERFACE_INCLUDE_DIRECTORIES "${ROOT_INCLUDE_DIRS}")

# Early ROOT does not include the flags required on targets
add_library(ROOT::Flags_CXX IMPORTED INTERFACE)


# ROOT 6.14 and earlier have a spacing bug in the linker flags
string(REPLACE "-L " "-L" ROOT_EXE_LINKER_FLAGS "${ROOT_EXE_LINKER_FLAGS}")

# Fix for ROOT_CXX_FLAGS not actually being a CMake list
separate_arguments(ROOT_CXX_FLAGS)
set_property(TARGET ROOT::Flags_CXX APPEND PROPERTY
    INTERFACE_COMPILE_OPTIONS ${ROOT_CXX_FLAGS})

# Add definitions
separate_arguments(ROOT_DEFINITIONS)
foreach(_flag ${ROOT_EXE_LINKER_FLAG_LIST})
    # Remove -D or /D if present
    string(REGEX REPLACE [=[^[-//]D]=] "" _flag ${_flag})
    set_property(TARGET ROOT::Flags APPEND PROPERTY INTERFACE_LINK_LIBRARIES ${_flag})
endforeach()

# This also fixes a bug in the linker flags
separate_arguments(ROOT_EXE_LINKER_FLAGS)
set_property(TARGET ROOT::Flags_CXX APPEND PROPERTY
    INTERFACE_LINK_LIBRARIES ${ROOT_EXE_LINKER_FLAGS})

# Make sure you link with ROOT::Flags_CXX too!
```

