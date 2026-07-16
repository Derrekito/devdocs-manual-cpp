# std::rename

```cpp
int rename( const char* old_filename, const char* new_filename );
```

Changes the filename of a file. The file is identified by character string
pointed to by `old_filename`. The new filename is identified by character string
pointed to by `new_filename`.

If `new_filename` exists, the behavior is implementation-defined.

### Parameters

- **old_filename** — pointer to a null-terminated string containing the path
  identifying the file to rename
- **new_filename** — pointer to a null-terminated string containing the new path
  of the file

### Return value

`​0​` upon success or non-zero value on error.

### Notes

POSIX specifies many additional details on the semantics of this function, which
are reproduced in C++ by `std::filesystem::rename`.

### Example

```cpp
#include <cstdio>
#include <cstdlib>
#include <fstream>
#include <iostream>

int main()
{
    if (!std::ofstream("from.txt").put('a')) // create and write to file
    {
        std::perror("Error creating from.txt");
        return EXIT_FAILURE;
    }

    if (std::rename("from.txt", "to.txt"))
    {
        std::perror("Error renaming");
        return EXIT_FAILURE;
    }

    std::cout << std::ifstream("to.txt").rdbuf() << '\n'; // print file
    return EXIT_SUCCESS;
}
```

Output:

```text
a
```

### See also

- **rename (C++17)** — moves or renames a file or directory (function)
- **remove** — erases a file (function)

**C documentation for `rename`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/rename*
