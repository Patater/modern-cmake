# GoogleTest: Submodule method (preferred)

To use this method, just checkout GoogleTest as a submodule:[^1]

```cmake
git submodule add --branch=release-1.8.0 ../../google/googletest.git extern/googletest
```

Then, in your main `CMakeLists.txt`:

```cmake
option(PACKAGE_TESTS "Build the tests" ON)
if(PACKAGE_TESTS)
    enable_testing()
    add_subdirectory(tests)
endif()
```

As mentioned before, you have to do the `enable_testing` in your main CMakeLists. Now, in your tests directory:

```cmake
add_subdirectory("${PROJECT_SOURCE_DIR}/extern/googletest" "extern/googletest")
```

If you did this in your main CMakeLists, you could use a normal add_subdirectory; the extra path here is needed to correct the build path because we are calling it from a subdirectory.

The next line is optional, but keeps your `CACHE` cleaner:

```cmake
mark_as_advanced(
    BUILD_GMOCK BUILD_GTEST BUILD_SHARED_LIBS
    gmock_build_tests gtest_build_samples gtest_build_tests
    gtest_disable_pthreads gtest_force_shared_crt gtest_hide_internal_symbols
)
```

If you are interested in keeping IDEs that support folders clean, I would also add these lines:

```cmake
set_target_properties(gtest PROPERTIES FOLDER extern)
set_target_properties(gtest_main PROPERTIES FOLDER extern)
set_target_properties(gmock PROPERTIES FOLDER extern)
set_target_properties(gmock_main PROPERTIES FOLDER extern)
```

Then, to add a test, I'd recommend the following macro:

```cmake
macro(package_add_test TESTNAME)
    add_executable(${TESTNAME} ${ARGN})
    target_link_libraries(${TESTNAME} gtest gmock gtest_main)
    add_test(${TESTNAME} ${TESTNAME})
    set_target_properties(${TESTNAME} PROPERTIES FOLDER tests)
endmacro()

package_add_test(test1 test1.cpp)
```

This will allow you to quickly and simply add tests. Feel free to adjust to suit your needs. If you haven't seen it before, `ARGN` is "every argument after the listed ones".


[^1]: Here I've assumed that you are working on a GitHub repository by using the relative path to googletest.


[CLIUtils/cmake]:  https://github.com/CLIUtils/cmake
[GoogleTest]:      https://github.com/google/googletest
[DownloadProject]: https://github.com/Crascit/DownloadProject
