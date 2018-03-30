# Testing

## General Testing Information

In your main CMakeLists.txt you need to add the following function call (not in a subfolder):

```cmake
enable_testing()
```

You can register targets with:

```cmake
add_test(NAME TestName COMMAND TargetName)
```

If you put something else besides a target name after COMMAND, it will register as a command line to run. It would also be valid to put the generator expression:

```cmake
add_test(NAME TestName COMMAND $<TARGET_FILE:${TESTNAME}>)
```

which would use the output location (thus, the executable) of the produced target.


Look at the subchapters for recipes for popular frameworks.
