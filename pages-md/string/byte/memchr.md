# std::memchr

```cpp
const void* memchr( const void* ptr, int ch, std::size_t count );
void* memchr(       void* ptr, int ch, std::size_t count );
```

Converts `ch` to `unsigned char` and locates the first occurrence of that value
in the initial `count` bytes (each interpreted as `unsigned char`) of the object
pointed to by `ptr`.

This function behaves as if it reads the bytes sequentially and stops as soon as
a matching bytes is found: if the array pointed to by `ptr` is smaller than
`count`, but the match is found within the array, the behavior is well-defined.
*(since C++17)*

### Parameters

- **ptr** — pointer to the object to be examined
- **ch** — byte to search for
- **count** — max number of bytes to examine

### Return value

Pointer to the location of the byte, or a null pointer if no such byte is found.

### Example

Search an array of characters.

```cpp
#include <cstring>
#include <iostream>

int main()
{
    char arr[] = {'a', '\0', 'a', 'A', 'a', 'a', 'A', 'a'};
    char *pc = (char*) std::memchr(arr, 'A', sizeof arr);
    if (pc != nullptr)
        std::cout << "search character found\n";
    else
        std::cout << "search character not found\n";
}
```

Output:

```text
search character found
```

### See also

- **strchr** — finds the first occurrence of a character (function)
- **findfind_iffind_if_not (C++11)** — finds the first element satisfying
  specific criteria (function template)

**C documentation for `memchr`**

---
*Source: https://en.cppreference.com/w/cpp/string/byte/memchr*
