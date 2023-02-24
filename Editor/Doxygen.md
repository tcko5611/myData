# Doxygen

# Basic usage

-   generate a config file
    
    ```bash
    $ doxygen -g Doxygen
    
    ```
    
-   change the Dosygen to generate all(include functions)
    
    ```
    EXTRACT_ALL            = NO (to YES)
    
    ```
    
-   generate document
    
    ```bash
    $ doxygen Doxygen
    $ cd latex
    $ make
    $ gvfs-open refmax.pdf
    $ cd ../html
    $ firefox index.html
    
    ```
    

# Basic style

## for class

```cpp
/**
  class description
*/

```

## for function

```cpp
/**
  function description
  @param param1 Param1 description
  @param param2 Param2 description
  @return Return description
*/

```

## for variable
```cpp
int a /**< variable description */

```