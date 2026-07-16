# std::wmemset

```cpp
wchar_t* wmemset( wchar_t* dest, wchar_t ch, std::size_t count );
```

Copies the wide character `ch` into each of the first `count` wide characters of
the wide character array pointed to by `dest`.

If overflow occurs, the behavior is undefined.

If `count` is zero, the function does nothing.

### Parameters

- **dest** — pointer to the wide character array to fill
- **ch** — fill wide character
- **count** — number of wide characters to fill

### Return value

Returns a copy of `dest`.

### Notes

This function is not locale-sensitive and pays no attention to the values of the
`wchar_t` objects it writes: nulls as well as invalid wide characters are
written too.

### Example

```cpp
#include <clocale>
#include <cwchar>
#include <iostream>
#include <locale>

int main()
{
    wchar_t ar[4] = {L'1', L'2', L'3', L'4'};
    std::wmemset(ar, L'\U0001f34c', 2); // replaces [12] with the 🍌 bananas
    std::wmemset(ar + 2, L'蕉', 2); // replaces [34] with the 蕉 bananas

    std::setlocale(LC_ALL, "en_US.utf8");
    std::wcout.imbue(std::locale("en_US.utf8"));
    std::wcout << std::wstring(ar, 4) << '\n';
}
```

Possible output:

```text
🍌🍌蕉蕉
```

### See also

- **memset** — fills a buffer with a character (function)
- **wmemcpy** — copies a certain amount of wide characters between two
  non-overlapping arrays (function)
- **fill_n** — copy-assigns the given value to N elements in a range (function
  template)

**C documentation for `wmemset`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wmemset*
