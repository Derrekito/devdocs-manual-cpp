# std::regex_traits<CharT>::length

```cpp
static std::size_t length( const char_type* p );
```

Calculates the length of a null-terminated character sequence, that is, the
smallest `i` such that `p[i] == 0`.

Standard library specializations of `std::regex_traits` execute
`std::char_traits<CharT>::length(p);`.

### Parameters

- **p** — pointer to the first element of the null-terminated character sequence

### Return value

The length of the null-terminated character string.

### Example

```cpp
#include <iostream>
#include <regex>

int main()
{
    std::cout << std::regex_traits<char>::length("Кошка") << '\n'
              << std::regex_traits<wchar_t>::length(L"Кошка") << '\n';
}
```

Output:

```text
10
5
```

---
*Source: https://en.cppreference.com/w/cpp/regex/regex_traits/length*
