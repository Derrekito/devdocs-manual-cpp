# std::strpbrk

```cpp
const char* strpbrk( const char* dest, const char* breakset );
char* strpbrk(       char* dest, const char* breakset );
```

Scans the null-terminated byte string pointed to by `dest` for any character
from the null-terminated byte string pointed to by `breakset`, and returns a
pointer to that character.

### Parameters

- **dest** — pointer to the null-terminated byte string to be analyzed
- **breakset** — pointer to the null-terminated byte string that contains the
  characters to search for

### Return value

Pointer to the first character in `dest`, that is also in `breakset`, or null
pointer if no such character exists.

### Notes

The name stands for "string pointer break", because it returns a pointer to the
first of the separator ("break") characters.

### Example

```cpp
#include <cstring>
#include <iomanip>
#include <iostream>

int main()
{
    const char* str = "hello world, friend of mine!";
    const char* sep = " ,!";

    unsigned int cnt = 0;
    do
    {
        str = std::strpbrk(str, sep); // find separator
        std::cout << std::quoted(str) << '\n';
        if (str)
            str += std::strspn(str, sep); // skip separator
        ++cnt; // increment word count
    } while (str && *str);

    std::cout << "There are " << cnt << " words\n";
}
```

Output:

```text
" world, friend of mine!"
", friend of mine!"
" of mine!"
" mine!"
"!"
There are 5 words
```

### See also

- **strcspn** — returns the length of the maximum initial segment that consists
  of only the characters not found in another byte string (function)
- **strtok** — finds the next token in a byte string (function)
- **strchr** — finds the first occurrence of a character (function)
- **wcspbrk** — finds the first location of any wide character in one wide
  string, in another wide string (function)

**C documentation for `strpbrk`**

---
*Source: https://en.cppreference.com/w/cpp/string/byte/strpbrk*
