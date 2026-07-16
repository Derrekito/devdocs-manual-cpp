# std::codecvt<InternT,ExternT,StateT>::encoding, do_encoding

```cpp
public:
int encoding() const throw();  // (until C++11)
public:
int encoding() const noexcept;  // (since C++11)
protected:
virtual int do_encoding() const throw();  // (until C++11)
protected:
virtual int do_encoding() const noexcept;  // (since C++11)
```

1) Public member function, calls the member function `do_encoding` of the most
   derived class.

2) If the encoding represented by this codecvt facet maps each internal
   character to the same, constant number of external characters, returns that
   number. If the encoding is variable-length (e.g. UTF-8 or UTF-16), returns
   `​0​`. If the encoding is state-dependent, returns `-1`.

### Return value

The exact number of `externT` characters that correspond to one `internT`
character, if constant. `​0​` if the number varies, `-1` if the encoding is
state-dependent.

The non-converting specialization `std::codecvt<char, char, std::mbstate_t>`
returns `1`.

### Example

```cpp
#include <iostream>
#include <locale>

int main()
{
    std::cout << "en_US.utf8 is a variable-length encoding, encoding() returns "
              << std::use_facet<std::codecvt<wchar_t, char, std::mbstate_t>>(
                     std::locale("en_US.utf8")
                 ).encoding() << '\n';

    std::cout << "zh_CN.gb18030 is also variable-length, encoding() == "
              << std::use_facet<std::codecvt<wchar_t, char, std::mbstate_t>>(
                     std::locale("zh_CN.gb18030")
                 ).encoding() << '\n';

    std::cout << "ru_RU.koi8r is a single-byte encoding, encoding() == "
              << std::use_facet<std::codecvt<wchar_t, char, std::mbstate_t>>(
                     std::locale("ru_RU.koi8r")
                 ).encoding() << '\n';
}
```

Output:

```text
en_US.utf8 is a variable-length encoding, encoding() returns 0
zh_CN.gb18030 is also variable-length, encoding() == 0
ru_RU.koi8r is a single-byte encoding, encoding() == 1
```

### See also

- **MB_CUR_MAX** — maximum number of bytes in a multibyte character in the
  current C locale (macro variable)
- **do_max_length [virtual]** — returns the maximum number of `ExternT`
  characters that could be converted into a single `InternT` character (virtual
  protected member function)

---
*Source: https://en.cppreference.com/w/cpp/locale/codecvt/encoding*
