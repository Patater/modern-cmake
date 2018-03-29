# OpenMP

[OpenMP] support was drastically improved in CMake 3.9+. The Modern(TM) way to add OpenMP to a target is:


```cmake
find_package(OpenMP)
if(OpenMP_CXX_FOUND)
    target_link_libraries(MyTarget PUBLIC OpenMP::OpenMP_CXX)
endif()
```

This not only is cleaner than the old method, it will also correctly set the library link line differently from the compile line if needed. However, if you need to support older CMake, the following works on CMake 3.1+:


```cmake
# For CMake < 3.9, we need to make the target ourselves
if(NOT TARGET OpenMP::OpenMP_CXX)
    add_library(OpenMP::OpenMP_CXX IMPORTED INTERFACE)
    set_property(TARGET OpenMP::OpenMP_CXX
                 PROPERTY INTERFACE_COMPILE_OPTIONS ${OpenMP_CXX_FLAGS})
    # Only works if the same flag is passed to the linker; use CMake 3.9+ otherwise (Intel, AppleClang)
    set_property(TARGET OpenMP::OpenMP_CXX
                 PROPERTY INTERFACE_LINK_LIBRARIES ${OpenMP_CXX_FLAGS})

    find_package(Threads REQUIRED)
    target_link_libraries(MyTarget INTERFACE Threads::Threads)
endif()
target_link_libraries(MyTarget PUBLIC OpenMP::OpenMP_CXX)
```

[OpenMP]: https://cmake.org/cmake/help/latest/module/FindOpenMP.html
