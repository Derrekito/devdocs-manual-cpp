# std::wcsxfrm

```cpp
std::size_t wcsxfrm( wchar_t* dest, const wchar_t* src, std::size_t count );
```

Transforms the null-terminated wide string pointed to by `src` into the
implementation-defined form such that comparing two transformed strings with
`std::wcscmp` gives the same result as comparing the original strings with
`std::wcscoll`, in the current C locale.

The first `count` characters of the transformed string are written to
destination, including the terminating null character, and the length of the
full transformed string is returned, excluding the terminating null character.

If `count` is `​0​`, then `dest` is allowed to be a null pointer.

### Notes

The correct length of the buffer that can receive the entire transformed string
is `1 + std::wcsxfrm(nullptr, src, 0)`.

This function is used when making multiple locale-dependent comparisons using
the same wide string or set of wide strings, because it is more efficient to use
`std::wcsxfrm` to transform all the strings just once, and subsequently compare
the transformed wide strings with `std::wcscmp`.

### Parameters

- **dest** — pointer to the first element of a wide null-terminated string to
  write the transformed string to
- **src** — pointer to the null-terminated wide character string to transform
- **count** — maximum number of characters to output

### Return value

The length of the transformed wide string, not including the terminating
null-character.

### Example

```cpp
#include <cwchar>
#include <iostream>

int main()
{
    std::setlocale(LC_ALL, "sv_SE.utf8");

    std::wstring in1 = L"\u00e5r";
    std::wstring out1(1 + std::wcsxfrm(nullptr, in1.c_str(), 0), L' ');
    std::wstring in2 = L"\u00e4ngel";
    std::wstring out2(1 + std::wcsxfrm(nullptr, in2.c_str(), 0), L' ');

    std::wcsxfrm(&out1[0], in1.c_str(), out1.size());
    std::wcsxfrm(&out2[0], in2.c_str(), out2.size());

    std::wcout << "In the Swedish locale: ";
    if (out1 < out2)
        std::wcout << in1 << " before " << in2 << '\n';
    else
        std::wcout << in2 << " before " << in1 << '\n';

    std::wcout << "In lexicographical comparison: ";
    if (in1 < in2)
        std::wcout << in1 << " before " << in2 << '\n';
    else
        std::wcout << in2 << " before " << in1 << '\n';

}
```

Output:

```text
In the Swedish locale: år before ängel
In lexicographical comparison: ängel before år
```

### See also

- **strxfrm** — transform a string so that `strcmp` would produce the same
  result as `strcoll` (function)
- **do_transform [virtual]** — transforms a string so that collation can be
  replaced by comparison (virtual protected member function of
  `std::collate<CharT>`)
- **wcscoll** — compares two wide strings in accordance to the current locale
  (function)

**C documentation for `wcsxfrm`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wcsxfrm*
