# std::strchr

```cpp
const char* strchr( const char* str, int ch );
char* strchr(       char* str, int ch );
```

Finds the first occurrence of the character `static_cast<char>(ch)` in the byte
string pointed to by `str`.

The terminating null character is considered to be a part of the string and can
be found if searching for `'\0'`.

### Parameters

- **str** — pointer to the null-terminated byte string to be analyzed
- **ch** — character to search for

### Return value

Pointer to the found character in `str`, or a null pointer if no such character
is found.

### Example

```cpp
#include <cstring>
#include <iostream>

int main()
{
    const char* str = "Try not. Do, or do not. There is no try.";
    char target = 'T';
    const char* result = str;

    while ((result = std::strchr(result, target)) != nullptr)
    {
        std::cout << "Found '" << target
                  << "' starting at '" << result << "'\n";

        // Increment result, otherwise we'll find target at the same location
        ++result;
    }
}
```

Output:

```text
Found 'T' starting at 'Try not. Do, or do not. There is no try.'
Found 'T' starting at 'There is no try.'
```

### See also

- **memchr** — searches an array for the first occurrence of a character
  (function)
- **find** — finds the first occurrence of the given substring (public member
  function of `std::basic_string<CharT,Traits,Allocator>`)
- **wcschr** — finds the first occurrence of a wide character in a wide string
  (function)
- **strrchr** — finds the last occurrence of a character (function)
- **strpbrk** — finds the first location of any character from a set of
  separators (function)

**C documentation for `strchr`**

---
*Source: https://en.cppreference.com/w/cpp/string/byte/strchr*
