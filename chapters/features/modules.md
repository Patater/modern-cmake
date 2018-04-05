# Useful Modules

There are a ton of [modules] in CMake; but some of them are more useful than others. Here are a few highlights.

## [CMakeDependentOption]

This adds a command `cmake_dependent_option` that sets an option based on another set of variables being true. It looks like this:

```cmake
include(CMakeDependentOption)
cmake_dependent_option(BUILD_TESTS "Build your tests" ON "VAL1;VAL2" OFF)
```

which is just a shortcut for this:

```cmake
if(VAL1 AND VAL2)
    set(BUILD_TESTS_DEFAULT ON)
else()
    set(BUILD_TESTS_DEFAULT OFF)
endif()

option(BUILD_TESTS "Build your tests" ${BUILD_TESTS_DEFAULT})

if(NOT BUILD_TESTS_DEFAULT)
    mark_as_advanced(BUILD_TESTS)
endif()
```

## [CheckCXXCompilerFlag]

This checks to see if a flag is supported. For example:

```cmake
include(CheckCXXCompilerFlag)
check_cxx_compiler_flag(-someflag OUPUT_VARIABLE)
```

Note that `OUTPUT_VARIABLE` will also appear in the configuration printout, so choose a good name.

This is just one of many similar modules, such as `CheckIncludeFileCXX`, `CheckStructHasMember`, `TestBigEndian`, and `CheckTypeSize` that allow you
to check for information about the system (and you can communicate that to your source code).

## [WriteCompilerDetectionHeader]

This is an amazing module similar to the ones listed above, but special enough to deserve its own section. It allows you
to look for a list of features that some compilers support, and write out a C++ header file that lets you know whether that
feature is available. It even can provide compatibility macros for features that have changed names!

To use:

```cmake
write_compiler_detection_header(
  FILE myoutput.h
  PREFIX My
  COMPILERS GNU Clang MSVC Intel
  FEATURES cxx_variadic_templates
)
```

This supports compiler features (defined to 0 or 1), symbols (defined to empty or the symbol), and macros that
support different names. They will be prefixed with the PREFIX you provide. You can separate compilers into different
files using `OUTPUT_FILES_DIR

The downside is that you do have to list the compilers you expect to support. If you use the `ALLOW_UNKNOWN_COMPILERS` flag(s),
you can keep this from erroring on unknown compilers, but it will still leave all features empty.

## [try_compile](https://cmake.org/cmake/help/latest/command/try_compile.html)/[try_run](https://cmake.org/cmake/help/latest/command/try_run.html)

This is not exactly a module, but is crutial to many of the modules listed above. You can attept to compile (and possibly run) a bit of code at configure time. This can allow you to get information about the capabilities of your system. The basic syntax is:

```cmake
try_compile(
  RESULT_VAR
    bindir
  SOURCES
    source.cpp
) 
```

There are lots of options you can add, like `COMPILE_DEFINITIONS`. In CMake 3.8+, this will honor the CMake C/C++/CUDA standard settings. If you use `try_run` instead, it will run the resulting program and give you the output in `RUN_OUTPUT_VARIABLE`.



[modules]: https://cmake.org/cmake/help/latest/manual/cmake-modules.7.html
[CMakeDependentOption]: https://cmake.org/cmake/help/latest/module/CMakeDependentOption.html
[CheckCXXCompilerFlag]: https://cmake.org/cmake/help/latest/module/CheckCXXCompilerFlag.html
[WriteCompilerDetectionHeader]: https://cmake.org/cmake/help/latest/module/WriteCompilerDetectionHeader.html
