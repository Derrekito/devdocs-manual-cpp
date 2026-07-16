# std::strcmp

```cpp
int strcmp( const char* lhs, const char* rhs );
```

Compares two null-terminated byte strings lexicographically.

The sign of the result is the sign of the difference between the values of the
first pair of characters (both interpreted as `unsigned char`) that differ in
the strings being compared.

The behavior is undefined if `lhs` or `rhs` are not pointers to null-terminated
strings.

### Parameters

- **lhs, rhs** — pointers to the null-terminated byte strings to compare

### Return value

Negative value if `lhs` appears before `rhs` in lexicographical order.

Zero if `lhs` and `rhs` compare equal.

Positive value if `lhs` appears after `rhs` in lexicographical order.

### Example

```cpp
#include <algorithm>
#include <cstring>
#include <iostream>
#include <vector>

int main()
{
    std::vector<const char*> cats{"Heathcliff", "Snagglepuss", "Hobbes", "Garfield"};
    std::sort(cats.begin(), cats.end(), [](const char* strA, const char* strB)
    {
        return std::strcmp(strA, strB) < 0;
    });

    for (const char* cat : cats)
        std::cout << cat << '\n';
}
```

Output:

```text
Garfield
Heathcliff
Hobbes
Snagglepuss
```

### See also

- **strncmp** — compares a certain number of characters from two strings
  (function)
- **wcscmp** — compares two wide strings (function)
- **memcmp** — compares two buffers (function)
- **strcoll** — compares two strings in accordance to the current locale
  (function)

**C documentation for `strcmp`**

---
*Source: https://en.cppreference.com/w/cpp/string/byte/strcmp*
