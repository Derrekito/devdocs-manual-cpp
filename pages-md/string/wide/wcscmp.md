# std::wcscmp

```cpp
int wcscmp( const wchar_t* lhs, const wchar_t* rhs );
```

Compares two null-terminated wide strings lexicographically.

The sign of the result is the sign of the difference between the values of the
first pair of wide characters that differ in the strings being compared.

The behavior is undefined if `lhs` or `rhs` are not pointers to null-terminated
wide strings.

### Parameters

- **lhs, rhs** — pointers to the null-terminated wide strings to compare

### Return value

Negative value if `lhs` appears before `rhs` in lexicographical order.

Zero if `lhs` and `rhs` compare equal.

Positive value if `lhs` appears after `rhs` in lexicographical order.

### Notes

This function is not locale-sensitive, unlike `std::wcscoll`, and the order may
not be meaningful when characters from different Unicode blocks are used
together or when the order of code units does not match collation order.

### Example

```cpp
#include <algorithm>
#include <cwchar>
#include <iostream>
#include <vector>

int main()
{
    std::vector<const wchar_t*> leaders{L"Ленин", L"Сталин", L"Маленков",
        L"Хрущёв", L"Брежнев", L"Андропов", L"Черненко", L"Горбачёв"};

    std::ranges::sort(leaders, [](auto leaderLHS, auto leaderRHS)
    {
        return std::wcscmp(leaderLHS, leaderRHS) < 0;
    });

    std::setlocale(LC_ALL, "en_US.utf8");
    std::wcout.imbue(std::locale("en_US.utf8"));
    for (auto leader : leaders)
        std::wcout << leader << '\n';
}
```

Possible output:

```text
Андропов
Брежнев
Горбачёв
Ленин
Маленков
Сталин
Хрущёв
Черненко
```

### See also

- **wcsncmp** — compares a certain amount of characters from two wide strings
  (function)
- **wmemcmp** — compares a certain amount of wide characters from two arrays
  (function)
- **strcmp** — compares two strings (function)
- **wcscoll** — compares two wide strings in accordance to the current locale
  (function)

**C documentation for `wcscmp`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wcscmp*
