# std::strcspn

```cpp
std::size_t strcspn( const char *dest, const char *src );
```

Returns the length of the maximum initial segment of the byte string pointed to
by `dest`, that consists of only the characters *not* found in byte string
pointed to by `src`.

The function name stands for "complementary span".

### Parameters

- **dest** — pointer to the null-terminated byte string to be analyzed
- **src** — pointer to the null-terminated byte string that contains the
  characters to search for

### Return value

The length of the maximum initial segment that contains only characters not
found in the byte string pointed to by `src`.

### Example

```cpp
#include <cstddef>
#include <cstring>
#include <iomanip>
#include <iostream>
#include <string>

int main()
{
    std::string s = "abcde312$#@";
    const char* invalid = "*$#";

    const std::size_t valid_len = std::strcspn(s.c_str(), invalid);
    if (valid_len != s.size())
    {
        std::cout << std::quoted(s)
                  << " contains invalid chars starting at position "
                  << valid_len << '\n'
                  << std::string(valid_len + 1, '-') << "^\n";
    }
}
```

Output:

```text
"abcde312$#@" contains invalid chars starting at position 8
---------^
```

### See also

- **strspn** — returns the length of the maximum initial segment that consists
  of only the characters found in another byte string (function)
- **wcscspn** — returns the length of the maximum initial segment that consists
  of only the wide *not* found in another wide string (function)
- **strpbrk** — finds the first location of any character from a set of
  separators (function)
- **find_first_of** — find first occurrence of characters (public member
  function of `std::basic_string<CharT,Traits,Allocator>`)

**C documentation for `strcspn`**

---
*Source: https://en.cppreference.com/w/cpp/string/byte/strcspn*
