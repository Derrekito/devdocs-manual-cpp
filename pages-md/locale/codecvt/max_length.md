# std::codecvt<InternT,ExternT,StateT>::max_length, do_max_length

```cpp
public:
int max_length() const throw();  // (until C++11)
public:
int max_length() const noexcept;  // (since C++11)
protected:
virtual int do_max_length() const throw();  // (until C++11)
protected:
virtual int do_max_length() const noexcept;  // (since C++11)
```

1) Public member function, calls the member function `do_max_length` of the most
   derived class.

2) Returns the maximum value that `do_length(state, from, from_end, 1)` can
   return for any valid range `[``from``,``from_end``)` and any valid `state`.

### Return value

The maximum number of `ExternT` characters that could be consumed if converted
by `in()` to produce one `InternT` character.

The non-converting specialization `std::codecvt<char, char, std::mbstate_t>`
returns `1`.

### Notes

If the encoding is state-dependent (`encoding() == -1`), then more than
`max_length()` external characters may be consumed to produce one internal
character.

### Example

```cpp
#include <codecvt>
#include <iostream>
#include <locale>

int main()
{
    std::cout << "In codecvt_utf8, the longest multibyte character is "
              << std::codecvt_utf8<wchar_t>().max_length() << " bytes long\n";

    std::cout << "In header-consuming codecvt_utf8, the longest multibyte character is "
              << std::codecvt_utf8<wchar_t,
                                   0x10ffff,
                                   std::consume_header>().max_length() << " bytes long\n";

    std::cout << "In this system's en_US.utf8, the longest multibyte character is "
              << std::use_facet<std::codecvt<wchar_t, char, std::mbstate_t>>(
                     std::locale("en_US.utf8")
                 ).max_length() << " bytes long\n";

    std::cout << "In this system's zh_CN.gb18030, the longest multibyte character is "
              << std::use_facet<std::codecvt<wchar_t, char, std::mbstate_t>>(
                     std::locale("zh_CN.gb18030")
                 ).max_length() << " bytes long\n";
}
```

Output:

```text
In codecvt_utf8, the longest multibyte character is 4 bytes long
In header-consuming codecvt_utf8, the longest multibyte character is 7 bytes long
In this system's en_US.utf8, the longest multibyte character is 6 bytes long
In this system's zh_CN.gb18030, the longest multibyte character is 4 bytes long
```

### See also

- **MB_CUR_MAX** — maximum number of bytes in a multibyte character in the
  current C locale (macro variable)
- **do_encoding [virtual]** — returns the number of `ExternT` characters
  necessary to produce one `InternT` character, if constant (virtual protected
  member function)

---
*Source: https://en.cppreference.com/w/cpp/locale/codecvt/max_length*
