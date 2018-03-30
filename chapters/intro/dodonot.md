# Do's and Don'ts

## CMake Version Choice

You'll need to pick a minimum required version of CMake. This will affect the CMake policies, and you should only use the features available in that version. Here are some common build environments and the CMake version you'll find on them. Feel free to install CMake yourself, it's 1-2 lines and there's nothing "special" about the built in version. It's also very backward compatible.

| Distribution  | CMake version | Notes |
|---------------|---------------|-------|
| RHEL/CentOS 7 | 2.8.11        | Don't use the default on this system. Grab a new copy or use the EPEL repo. |
| EPEL for RHEL/CentOS | 3.6.3    | Called `cmake3` |
| Ubuntu 14.04 LTS | 2.8.12 | Don't use the default on this system. |
| Ubuntu 16.04 LTS | 3.5.1 | |
| Ubuntu 17.10 | 3.9.1 | |
| Ubuntu 18.04 LTS | 3.10.2 | |
| Python PyPI  | 3.10 | Just `pip install cmake` on many systems. Add `--user` for local installs. |
| Homebrew on macOS | 3.11 | On macOS with Homebrew, this is only a few minutes behind cmake.org. |
| Chocolaty on Windows | 3.11 | Also up to date. The normal cmake.org installers are common on Windows, as well. |
| TravisCI | 3.9 | The December 2017 update added a recent version of clang and CMake! Finally! |

## CMake Antipatterns

The next two lists are heavily based on the excellent gist [Effective Modern CMake]. That list is much longer and more detailed, feel free to read it as well.

* **Do not use global functions**: This includes `link_directories`, `include_libraries`, and similar.
* **Don't add unneeded PUBLIC requirements**: You should avoid forcing something on users that is not required (`-Wall`). Make these PRIVATE instead.
* **Don't GLOB files**: Make or another tool will not know if you add files without rerunning CMake.
* **Link to built files directly**: Always link to targets if available.
* **Never skip PUBLIC/PRIVATE when linking**: This causes all future linking to be keyword-less. 


## CMake Patterns

* **Treat CMake as code**: It is code. It should be as clean and readable as all other code.
* **Think in targets**: Your targets should represent concepts. Make an (IMPORTED) INTERFACE target for anything that should stay together and link to that.
* **Export your interface**: You should be able to run from build or install.
* **Write a Config.cmake file**: This is what a library author should do to support clients.
* **Make ALIAS targets to keep usage consistent**: Using `add_subdirectory` and `find_package` should provide the same targets and namespaces.
* **Combine common functionality into clearly documented functions or macros**: Functions are better usually.




[Effective Modern CMake]: https://gist.github.com/mbinna/c61dbb39bca0e4fb7d1f73b0d66a4fd1
