# std::strlen

```cpp
std::size_t strlen( const char* str );
```

Returns the length of the given byte string, that is, the number of characters
in a character array whose first element is pointed to by `str` up to and not
including the first null character. The behavior is undefined if there is no
null character in the character array pointed to by `str`.

### Parameters

- **str** — pointer to the null-terminated byte string to be examined

### Return value

The length of the null-terminated string `str`.

### Possible implementation

```cpp
std::size_t strlen(const char* start)
{
    // NB: start is not checked for nullptr!
    const char* end = start;
    while (*end != '\0')
        ++end;
    return end - start;
}
```

### Example

```cpp
#include <cstring>
#include <iostream>

int main()
{
    const char str[] = "dog cat\0mouse";

    std::cout << "without null character: " << std::strlen(str) << '\n'
              << "with null character: " << sizeof str << '\n';
}
```

Output:

```text
without null character: 7
with null character: 14
```

### See also

- **wcslen** — returns the length of a wide string (function)
- **mblen** — returns the number of bytes in the next multibyte character
  (function)

**C documentation for `strlen`**

---
*Source: https://en.cppreference.com/w/cpp/string/byte/strlen*
