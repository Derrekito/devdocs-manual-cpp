# std::wmemmove

```cpp
wchar_t* wmemmove( wchar_t* dest, const wchar_t* src, std::size_t count );
```

Copies exactly `count` successive wide characters from the wide character array
pointed to by `src` to the wide character array pointed to by `dest`.

If `count` is zero, the function does nothing.

The arrays may overlap: copying takes place as if the wide characters were
copied to a temporary wide character array and then copied from the temporary
array to `dest`.

### Parameters

- **dest** — pointer to the wide character array to copy to
- **src** — pointer to the wide character array to copy from
- **count** — number of wide characters to copy

### Return value

Returns a copy of `dest`.

### Notes

This function is not locale-sensitive and pays no attention to the values of the
`wchar_t` objects it copies: nulls as well as invalid characters are copied too.

### Example

```cpp
#include <clocale>
#include <cwchar>
#include <iostream>
#include <locale>

int main()
{
    std::setlocale(LC_ALL, "en_US.utf8");
    std::wcout.imbue(std::locale("en_US.utf8"));

    wchar_t str[] = L"αβγδεζηθικλμνξοπρστυφχψω";
    std::wcout << str << '\n';
    std::wmemmove(str + 4, str + 3, 3); // copy from [δεζ] to [εζη]
    std::wcout << str << '\n';
}
```

Possible output:

```text
αβγδεζηθικλμνξοπρστυφχψω
αβγδδεζθικλμνξοπρστυφχψω
```

### See also

- **wmemcpy** — copies a certain amount of wide characters between two
  non-overlapping arrays (function)
- **memmove** — moves one buffer to another (function)
- **copycopy_if (C++11)** — copies a range of elements to a new location
  (function template)
- **copy_backward** — copies a range of elements in backwards order (function
  template)

**C documentation for `wmemmove`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wmemmove*
