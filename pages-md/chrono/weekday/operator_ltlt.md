# std::chrono::operator<<(std::chrono::weekday)

```cpp
template< class CharT, class Traits >
std::basic_ostream<CharT, Traits>&
    operator<<( std::basic_ostream<CharT, Traits>& os, const std::chrono::weekday& wd );  // (since C++20)
```

If `!wd.ok()`, inserts `wd.c_encoding()` followed by `" is not a valid weekday"`
into `os`. Otherwise, forms a `std::basic_string<CharT>` `s` consisting of the
abbreviated weekday name for the weekday represented by `wd`, determined using
the locale associated with `os`, and inserts `s` into `os`.

Equivalent to

`return os << (wd.ok() ? std::format(os.getloc(),
STATICALLY_WIDEN<CharT>("{:L%a}"), wd) : std::format(os.getloc(),
STATICALLY_WIDEN<CharT>("{} is not a valid weekday"), wd.c_encoding()));`

where `STATICALLY_WIDEN<CharT>("...")` is `"..."` if `CharT` is `char`, and
`L"..."` if `CharT` is `wchar_t`.

### Return value

`os`

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2372R3 | C++20 | the given locale was used by default | `L` is needed to use
      the given locale

### See also

- **format (C++20)** — stores formatted representation of the arguments in a new
  string (function template)
- **std::formatter<std::chrono::weekday> (C++20)** — formatting support for
  `weekday` (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/chrono/weekday/operator_ltlt*
