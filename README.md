# CMake instructions

This project uses [CUPID](https://github.com/lgpasquale/cupid) to manage
dependecies and to provide some utility functions

To update CUPID's version simply change the value of the variable
`CUPID_VERSION` in `CMakeLists.txt`

## Configuring the project

Edit the CMakeLists.txt in the main directory and set the project name and
version in the first few lines of the files

To add a dependency use the function `add_project_dependency`

## Adding Files to the library #

If you need to create a subdirectory in `src` simply edit `src/CMakeLists.txt`
and add:
```cmake
include_subdirectory(your_subdirectory_name)
```

To add source files to the library in a subdirectory of `src` edit (or create)
the file `CMakeLists.txt` in that subdirectory so that it contains something like:
```cmake
set(${PROJECT_NAME}_HEADERS ${${PROJECT_NAME}_HEADERS}
    your_subdirectory_name/your_header_file.hpp
    your_subdirectory_name/your_other_header_file.hpp
    CACHE INTERNAL "")

set(${PROJECT_NAME}_SOURCES ${${PROJECT_NAME}_SOURCES}
    your_subdirectory_name/your_source_file.cpp
    your_subdirectory_name/your_other_source_file.cpp
    CACHE INTERNAL "")
```

## Adding executables to the library #

To create an executable you can use the cmake macro `add_project_executable`.
Simply add in a CMakeLists.txt (preferably under `src/bin`):
```cmake
add_project_executable(target-name source1.cpp [source2.cpp ...])
```

## Adding tests to the library #

To create a test you can use the cmake macro `add_project_test_executable`.
Simply add in a CMakeLists.txt (under `src/test`):
```cmake
add_project_test_executable(target-name source1.cpp [source2.cpp ...])
```
