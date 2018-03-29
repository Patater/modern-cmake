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

Since `BOOL` is such a common variable type, you can set it more succinctly with the shortcut:

 ```cmake
 option(MY_OPTION "This is settable from the command line" OFF)
 ``` 
 
For the `BOOL` datatype, there are several different wordings for `ON` and `OFF`.

See [cmake-variables] for a listing of known variables in CMake.

## The Cache
 
The cache is actually just a text file, `CMakeCache.txt`, that gets created in the build directory when you run CMake. This is how CMake remembers anything you set, so you don't have to re-list your options every time you rerun CMake. 

## Properties

The other way CMake stores information is in properties. This is like a variable, but it is attached to some other item, like a directory or a target. A global property can be a useful uncached global variable. Many target properties are initialized from a matching variable with `CMAKE_` at the front. So setting `CMAKE_CXX_STANDARD`, for example, will mean that all new targets created will have `CXX_STANDARD` set to that when they are created. There are two
ways to set properties:

```cmake
set_property(TARGET TargetName
             PROPERTY CXX_STANDARD 11)

set_target_properties(TargetName PROPERTIES
                      CXX_STANDARD 11)
```

The first form is more general, and can set multiple targets/files/tests at once, and has useful options. The second is a shortcut for setting several properties on one target. And you can get targets similarly:

```cmake
get_property(ResultVariable TARGET TargetName PROPERTY CXX_STANDARD)
```

See [cmake-properties] for a listing of all known properties. You can also make your own in some cases.[^2]

[cmake-properties]: https://cmake.org/cmake/help/latest/manual/cmake-properties.7.html
[cmake-variables]: https://cmake.org/cmake/help/latest/manual/cmake-variables.7.html

[^1]: `if` statements are a bit odd in that they can take the variable with or without the surrounding syntax; this is there for historical reasons: `if` predates the `${}` syntax.
[^2]: Interface targets, for example, may have limits on custom properties that are allowed.
