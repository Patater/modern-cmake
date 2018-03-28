# ROOT

ROOT is a C++ Toolkit for High Energy Physics. It is huge. There are really a lot of ways to use it in CMake, though many/most of the examples you'll find are probably wrong. Here's my recommendation.


## Finding ROOT

ROOT supports config file discovery, so you can just do:

```cmake
find_package(ROOT CONFIG)
```

to attempt to find ROOT. If you don't have your paths set up, you can pass `-DROOT_DIR=$ROOTSYS/cmake` to find ROOT. (But, really, you should source `thisroot.sh`)

## The wrong way

ROOT provides a utility to set up a ROOT project, which you can activate using `include(${ROOT_USE_FILE})`. This will automatically make ugly global variables for you. Don't use it.

## The right way (Targets)

ROOT does not correctly set up it's imported targets. To fix this error, you'll need something like:

```cmake

```

In CMake 3.11, you can just do:

```cmake
```

