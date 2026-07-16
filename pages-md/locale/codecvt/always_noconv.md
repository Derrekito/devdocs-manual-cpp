# std::codecvt<InternT,ExternT,StateT>::always_noconv, do_always_noconv

```cpp
public:
bool always_noconv() const throw();  // (until C++11)
public:
bool always_noconv() const noexcept;  // (since C++11)
protected:
virtual bool do_always_noconv() const throw();  // (until C++11)
protected:
virtual bool do_always_noconv() const noexcept;  // (since C++11)
```

1) Public member function, calls the member function `do_always_noconv` of the
   most derived class.

2) Returns `true` if both `do_in()` and `do_out()` return
   `std::codecvt_base::noconv` for all valid inputs.

### Return value

`true` if this conversion facet performs no conversions, `false` otherwise.

The non-converting specialization `std::codecvt<char, char, std::mbstate_t>`
returns `true`.

### Notes

This function may be used e.g. in the implementation of
`std::basic_filebuf::underflow` and `std::basic_filebuf::overflow` to use bulk
character copy instead of calling `std::codecvt::in` or `std::codecvt::out` if
it is known that the locale imbued in the `std::basic_filebuf` does not perform
any conversions.

### Example

```cpp
#include <iostream>
#include <locale>

int main()
{
    std::cout << "The non-converting char<->char codecvt::always_noconv() returns "
              << std::boolalpha
              << std::use_facet<std::codecvt<char, char, std::mbstate_t>>(
                    std::locale()
                 ).always_noconv() << '\n'
              << "while wchar_t<->char codecvt::always_noconv() returns "
              << std::use_facet<std::codecvt<wchar_t, char, std::mbstate_t>>(
                    std::locale()
                 ).always_noconv() << '\n';
}
```

Output:

```text
The non-converting char<->char codecvt::always_noconv() returns true
while wchar_t<->char codecvt::always_noconv() returns false
```

---
*Source: https://en.cppreference.com/w/cpp/locale/codecvt/always_noconv*
