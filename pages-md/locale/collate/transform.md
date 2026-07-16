# std::collate<CharT>::transform, do_transform

```cpp
public:
string_type transform( const CharT* low, const CharT* high ) const;  // (1)
protected:
virtual string_type do_transform( const CharT* low, const CharT* high ) const;  // (2)
```

1) Public member function, calls the protected virtual member function
   `do_transform` of the most derived class.

2) Converts the character sequence `[``low``,``high``)` to a string that,
   compared lexicographically (e.g. with `operator<` for strings) with the
   result of calling `transform()` on another string, produces the same result
   as calling `do_compare()` on the same two strings.

### Parameters

- **low** — pointer to the first character in the sequence to transform
- **high** — one past the end pointer for the sequence to transform

### Return value

The string transformed so that lexicographic comparison of the transformed
strings may be used instead of collating of the originals. In the "C" locale,
the returned string is the exact copy of `[``low``,``high``)`. In other locales,
the contents of the returned string are implementation-defined, and the size may
be considerably longer.

### Notes

In addition to the use in collation, the implementation-specific format of the
transformed string is known to std::regex_traits<>::transform_primary, which is
able to extract the equivalence class information.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <locale>

int main()
{
    std::locale::global(std::locale("sv_SE.utf8"));
    auto& f = std::use_facet<std::collate<wchar_t>>(std::locale());

    std::wstring in1 = L"\u00e4ngel";
    std::wstring in2 = L"\u00e5r";

    std::wstring out1 = f.transform(&in1[0], &in1[0] + in1.size());
    std::wstring out2 = f.transform(&in2[0], &in2[0] + in2.size());

    std::wcout << "In the Swedish locale: ";
    if (out1 < out2)
        std::wcout << in1 << " before " << in2 << '\n';
    else
        std::wcout << in2 << " before " << in1 << '\n';

    std::wcout << "In lexicographic comparison: ";
    if (in1 < in2)
        std::wcout << in1 << " before " << in2 << '\n';
    else
        std::wcout << in2 << " before " << in1 << '\n';
}
```

Output:

```text
In the Swedish locale: år before ängel
In lexicographic comparison: ängel before år
```

### See also

- **strxfrm** — transform a string so that `strcmp` would produce the same
  result as `strcoll` (function)
- **wcsxfrm** — transform a wide string so that `wcscmp` would produce the same
  result as `wcscoll` (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/collate/transform*
