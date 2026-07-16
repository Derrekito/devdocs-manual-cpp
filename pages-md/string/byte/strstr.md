# std::strstr

```cpp
const char* strstr( const char* haystack, const char* needle );
char* strstr(       char* haystack, const char* needle );
```

Finds the first occurrence of the byte string `needle` in the byte string
pointed to by `haystack`. The terminating null characters are not compared.

### Parameters

- **haystack** — pointer to the null-terminated byte string to examine
- **needle** — pointer to the null-terminated byte string to search for

### Return value

Pointer to the first character of the found substring in `haystack`, or a null
pointer if no such character is found. If `needle` points to an empty string,
`haystack` is returned.

### Example

```cpp
#include <cstring>
#include <iostream>

int main()
{
    const char* str = "Try not. Do, or do not. There is no try.";
    const char* target = "not";
    const char* result = str;

    while ((result = std::strstr(result, target)))
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
Found 'not' starting at 'not. Do, or do not. There is no try.'
Found 'not' starting at 'not. There is no try.'
```

### See also

- **find** — finds the first occurrence of the given substring (public member
  function of `std::basic_string<CharT,Traits,Allocator>`)
- **wcsstr** — finds the first occurrence of a wide string within another wide
  string (function)
- **strchr** — finds the first occurrence of a character (function)
- **strrchr** — finds the last occurrence of a character (function)

**C documentation for `strstr`**

---
*Source: https://en.cppreference.com/w/cpp/string/byte/strstr*
