# Variables and the Cache

## Local Variables
We will cover variables first. A local variable is set like this:

```CMake
set(MY_VARIABLE "value")
```

The names of variables are usually all caps, and the value follows. You access a variable by using `${}`, such as `${MY_VARIABLE}`.[^1]  CMake has the concept of scope; you can access the value of the variable after you set it as long as you are in the same scope. If you leave a function or a file in a sub directory, the variable will no longer be defined. You can set a variable in the scope immediately above your current one with `PARENT_SCOPE` at the end.

## Cache Variables

If you want to set a variable from the command line, CMake offers a variable cache. Some variables are already here, like `CMAKE_BUILD_TYPE`. The syntax for declaring a variable and setting it if it is not already set is:

```cmake
set(MY_CACHE_VARIABLE "VALUE" CACHE STRING "Description")
```

This will **not** replace an existing value. This is so that you can set these on the command line and not have them overridden when the CMake file executes. If you want to use these variables as a make-shift global variable, then you can do:

```cmake
set(MY_CACHE_VARIABLE "VALUE" CACHE STRING "" FORCE)
mark_as_advanced(MY_CACHE_VARIABLE)
```

The first line will cause the value to be set no matter what, and the second line will keep the variable from showing up in the list of variables if you run `cmake -L ..` or use a GUI.

Since `BOOL` is such a common variable type, you can set it more susinctly with the shortcut:

 ```cmake
 option(MY_OPTION "This is settable from the command line" OFF)
 ``` 
 
 For the BOOL datatype, there are several different workings for `ON` and `OFF`.

[^1]: `if` statements are a bit odd in that they can take the variable with or without the surrounding syntax; this is there for historical reasons: `if` predates the `${}` syntax.
