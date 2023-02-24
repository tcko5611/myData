# CMake QT

# QT property

```cmake
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)
set(Qt5_DIR "/remote/customcm1/build_dependency/QSCQ/qt/5.6.2-42/linux64/shared/debug/lib/cmake/Qt5" CACHE PATH "Initial cache" FORCE)
```

## qt libray

```cmake
find_package(Qt5 COMPONENTS Core Widgets REQUIRED)
qt5_use_modules(test_serial Core Widgets)

```

## Cmake question

-   It seems like qt will link the Qt libraries direct to the binary.