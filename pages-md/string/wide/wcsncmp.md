# std::wcsncmp

```cpp
int wcsncmp( const wchar_t* lhs, const wchar_t* rhs, std::size_t count );
```

Compares at most `count` wide characters of two null-terminated wide strings.
The comparison is done lexicographically.

The sign of the result is the sign of the difference between the values of the
first pair of wide characters that differ in the strings being compared.

The behavior is undefined if `lhs` or `rhs` are not pointers to null-terminated
strings.

### Parameters

- **lhs, rhs** — pointers to the null-terminated wide strings to compare
- **count** — maximum number of characters to compare

### Return value

Negative value if `lhs` appears before `rhs` in lexicographical order.

Zero if `lhs` and `rhs` compare equal.

Positive value if `lhs` appears after `rhs` in lexicographical order.

### Example

```cpp
#include <clocale>
#include <cwchar>
#include <iostream>
#include <locale>

void demo(const wchar_t* lhs, const wchar_t* rhs, int sz)
{
    int rc = std::wcsncmp(lhs, rhs, sz);
    if (rc == 0)
        std::wcout << "First " << sz << " characters of ["
                   << lhs << "] equal [" << rhs << "]\n";
    else if (rc < 0)
        std::wcout << "First " << sz << " characters of ["
                   << lhs << "] precede [" << rhs << "]\n";
    else if (rc > 0)
        std::wcout << "First " << sz << " characters of ["
                   << lhs << "] follow [" << rhs << "]\n";
}

int main()
{
    const wchar_t str1[] = L"안녕하세요";
    const wchar_t str2[] = L"안녕히 가십시오";

    std::setlocale(LC_ALL, "en_US.utf8");
    std::wcout.imbue(std::locale("en_US.utf8"));
    demo(str1, str2, 5);
    demo(str2, str1, 8);
    demo(str1, str2, 2);
}
```

Output:

```text
First 5 characters of [안녕하세요] precede [안녕히 가십시오]
First 8 characters of [안녕히 가십시오] follow [안녕하세요]
First 2 characters of [안녕하세요] equal [안녕히 가십시오]
```

### See also

- **strncmp** — compares a certain number of characters from two strings
  (function)
- **wcscmp** — compares two wide strings (function)
- **wmemcmp** — compares a certain amount of wide characters from two arrays
  (function)
- **wcscoll** — compares two wide strings in accordance to the current locale
  (function)

**C documentation for `wcsncmp`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wcsncmp*
