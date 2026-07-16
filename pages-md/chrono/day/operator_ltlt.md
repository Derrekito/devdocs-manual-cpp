# std::chrono::operator<<(std::chrono::day)

```cpp
template< class CharT, class Traits >
std::basic_ostream<CharT, Traits>&
    operator<<( std::basic_ostream<CharT, Traits>& os, const std::chrono::day& d );  // (since C++20)
```

Forms a `std::basic_string<CharT>` `s` consisting of the day value stored in `d`
formatted as a decimal number, with a leading zero if the result would otherwise
be a single decimal digit. Then, if `!d.ok()`, append `" is not a valid day"` to
the formatted string. Inserts that string into `os`.

Equivalent to

`return os << (d.ok() ? std::format(STATICALLY_WIDEN<CharT>("{:%d}"), d) :
std::format(STATICALLY_WIDEN<CharT>("{:%d} is not a valid day"), d));`

where `STATICALLY_WIDEN<CharT>("...")` is `"..."` if `CharT` is `char`, and
`L"..."` if `CharT` is `wchar_t`.

### Return value

`os`

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    constexpr std::chrono::day d1{31}, d2{7}, d3{42}, d4{};
    std::cout << d1 << '\n'
              << d2 << '\n'
              << d3 << '\n'
              << d4 << '\n';
}
```

Possible output:

```text
31
07
42 is not a valid day
00 is not a valid day
```

### See also

- **format (C++20)** — stores formatted representation of the arguments in a new
  string (function template)
- **std::formatter<std::chrono::day> (C++20)** — formatting support for `day`
  (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/chrono/day/operator_ltlt*
