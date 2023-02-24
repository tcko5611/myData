# company example

# Sample CMakeLists file

```cmake
cmake_minimum_required(VERSION 3.2)
# the next lines must be before project
set(CMAKE_C_COMPILER /depot/qsc/QSCQ/bin/gcc)
set(CMAKE_CXX_COMPILER /depot/qsc/QSCQ/bin/g++)
project(example)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

set(ROOT_PATH "/slowfs/tw_rnd2/ktc/ktc_cd_q2020.03")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)
set(Qt5_DIR "/global/libs/qt_qscn_2017.09/5.6.2/linux64_debug_shlib/install/lib/cmake/Qt5" CACHE PATH "Initial cache" FORCE)
find_package(Qt5 COMPONENTS Core Widgets REQUIRED)
add_executable(customfault main.cpp StarDelegate.cpp StarDelegate.h
StarEditor.cpp StarEditor.h StarRating.cpp MyModel.h MyModel.cpp)
target_include_directories(customfault PUBLIC
  ${CMAKE_CURRENT_SOURCE_DIR}
  )
qt5_use_modules(customfault Core Widgets)
```

# Generate code
```bash
mkdir Debug
cd Debug
cmake -DCMAKE_BUILD_TYPE=Debug ..
make
```
# Run code
```
setenv LD_LIBRARY_PATH /depot/qsc/QSCT/GCC/lib64
```