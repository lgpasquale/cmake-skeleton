# Configuration

Configuration is handled through CMake.
To configure run cmake from the build directory:

```sh
cd /path/to/build/dir
cmake \
    -D CMAKE_BUILD_TYPE:STRING="Release" \
    -D CMAKE_INSTALL_PREFIX:PATH=/path/to/install/dir \
    /path/to/source/dir
```

where /path/to/source/dir is the directory containig this README

By default cmake will try to install all dependencies. If you want to use a version of a dependecy installed elsewhere, you can set the cmake variable
`DEPENDENCIES_INSTALL_<dependency name>` to off and then
use the variable `DEPENDENCIES_<dependency name>_DIR` to tell cmake where to find that dependency.
For example:

```sh
cd /path/to/build/dir
cmake \
    -D CMAKE_BUILD_TYPE:STRING="Release" \
    -D CMAKE_INSTALL_PREFIX:PATH=/path/to/install/dir \
    -D DEPENDENCIES_<library name>_DIR:PATH=/path/to/dependency/install/dir \
    -D DEPENDENCIES_INSTALL_<dependency name>:BOOL=OFF \
    /path/to/source/dir
```

To specify the type of build you can set the variable `CMAKE_BUILD_TYPE`
to one of the following:
- `Release`     compiles with flags -O2 -DNDEBUG
- `Debug`       compiles with flags -D_GLIBCXX_DEBUG -DDEBUG -g -O0
- `Profile`     compiles with flags -O2 -pg

## Build

To build everything run make from the build directory
```sh
cd /path/to/build/dir
make
```

To build documentation use the 'doc' target:
```sh
make doc
```

CMake generates different targes for every library, executable and test.
they are all included in the target "all" but can be built individually.
To list available targes:
```
cd /path/to/build/dir
make help
```

## Testing

To run all tests:
```sh
cd /path/to/build/dir
make
ctest
```

## Install

```sh
cd /path/to/build/dir
make install
```
