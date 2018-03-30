#FetchContent (CMake 3.11+)

Often, you would like to do your download of data or packages as part of the configure instead of the build. This was invented several times in third party modules, but was finally added to CMake itself as part of CMake 3.11 as the [FetchContent] module.

The [FetchContent] module has excellent documentation that I won't try to repeat. The key ideas are:

* Use `FetchContent_Declare(MyName` to get data or a package. You can set URLs, Git repositories, and more.
* Use `FetchContent_GetProperties(MyName)` on the name you picked in the first step to get `MyName_*` variables.
* Check `MyName_POPULATED`, and if not populated, use `FetchContent_Populate(MyName)` (and if a package, `add_subdirectory("${MyName_SOURCE_DIR}" "${MyName_SOURCE_DIR}")`) 

[FetchContent]: https://cmake.org/cmake/help/latest/module/FetchContent.html

{% hint style='working' %}
This document is a work in progress. You can raise an issue or put in a merge request on [GitLab](https://gitlab.com/CLIUtils/modern-cmake).
{% endhint %}
 ~
