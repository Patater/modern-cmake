# GoogleTest: Download method

You can use the downloader in my [CMake helper repository][CLIUtils/cmake], using CMake's `include` command.

This is a downloader for [GoogleTest], based on the excellent [DownloadProject] tool. Downloading a copy for each project is the recommended way to use GoogleTest (so much so, in fact, that they have disabled the automatic CMake install target), so this respects that design decision. This method downloads the project at configure time, so that IDE's correctly find the libraries. Using it is simple:

```cmake
cmake_minimum_required(VERSION 3.4)
project(MyProject CXX)
list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)

enable_testing() # Must be in main file

include(AddGoogleTest) # Could be in /tests/CMakeLists.txt
add_executable(SimpleTest SimpleTest.cu)
add_gtest(SimpleTest)
```

> Note: `add_gtest` is just a macro that adds `gtest`, `gmock`, and `gtest_main`, and then runs `add_test` to create a test with the same name:
> ```cmake
> target_link_libraries(SimpleTest gtest gmock gtest_main)
> add_test(SimpleTest SimpleTest)
> ```


## Catch


[^1]: Here I've assumed that you are working on a GitHub repository by using the relative path to googletest.


[CLIUtils/cmake]:  https://github.com/CLIUtils/cmake
[GoogleTest]:      https://github.com/google/googletest
[DownloadProject]: https://github.com/Crascit/DownloadProject
