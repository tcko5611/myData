# Static Library

Use the following commands to make object files and static library:

```
cc -Wall -c ctest1.c ctest2.c
ar -cvq libtests.a ctest1.o ctest2.o

```

Use the following command to build executable

```
cc -o testa prog.c -L. -lctest

```

for **cmake**, we can add the following command to CMakeLists.txt

```
add_library(tests test1.c test2.c)
add_executable(testa prog.c)
target_link_libraries(testa tests)

```

# Dynamic Library

Use the following commands to make object files and dynamic library:

```
cc -Wall -c -fPIC ctest1.c ctest2.c
gcc -shared -Wl,-soname,libctest.so.1 -o libctest.so.1.0 *.o
mv libctest.so.1.0 /opt/lib
ln -sf /opt/lib/libctest.so.1.0 /opt/lib/libctest.so.1
ln -sf /opt/lib/libctest.so.1.0 /opt/lib/libctest.so

```

for _-Wl,-soname,libctest.so.1_ is to indicate the dynamic link library is libctest.so.1 when execute. And lib _[libctest.so](http://libctest.so/)_ is covinient to build exectutable by using gcc.

Use following command to build executable

```
cc -o testb prog.c -L. -lctest

```

for **cmake**, we can add the following command to CMakeLists.txt

```
add_library(testd SHARED test1.c test2.c)
add_executable(testb prog.c)
target_link_libraries(testb testd)

```

# Dynamic Loading Library

You can use the dynamic libarary function by the following steps:

-   Build a dynamic library like [libctest.so](http://libctest.so/)
-   Use _lib_handle = dlopen("./libtestdl.dll", RTLD_LAZY);_ to open the lib
-   USe _fn = dlsym(lib_handle, "ctest1");_ to use the function in the dynamic library
-   Use _dlclose(lib_handle);_ to close the library.

# Metion about cmake

The above procedure for direct commands are suitable for **UNIX** like system. For _i386 PE_ executable, it's more complicate. But you can just use cmake to generate additional files to use in _i386 PE_ system