# std::strrchr

```cpp
const char* strrchr( const char* str, int ch );
char* strrchr(       char* str, int ch );
```

Finds the last occurrence of `ch` (after conversion to `char`) in the byte
string pointed to by `str`. The terminating null character is considered to be a
part of the string and can be found if searching for `'\0'`.

### Parameters

- **str** — pointer to the null-terminated byte string to be analyzed
- **ch** — character to search for

### Return value

Pointer to the found character in `str`, or null pointer if no such character is
found.

### Example

```cpp
#include <cstring>
#include <iostream>

int main()
{
    char input[] = "/home/user/hello.c";
    char* output = std::strrchr(input, '/');
    if (output)
        std::cout << output + 1 << '\n';
}
```

Output:

```text
hello.c
```

### See also

- **strchr** — finds the first occurrence of a character (function)
- **wcsrchr** — finds the last occurrence of a wide character in a wide string
  (function)
- **rfind** — find the last occurrence of a substring (public member function of
  `std::basic_string<CharT,Traits,Allocator>`)

**C documentation for `strrchr`**

---
*Source: https://en.cppreference.com/w/cpp/string/byte/strrchr*
