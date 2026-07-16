# std::chrono::operator<<(std::chrono::month)

```cpp
template< class CharT, class Traits >
std::basic_ostream<CharT, Traits>&
    operator<<( std::basic_ostream<CharT, Traits>& os, const std::chrono::month& m );  // (since C++20)
```

If `!m.ok()`, inserts `unsigned(m)` followed by `" is not a valid month"` to
`os`. Otherwise, forms a `std::basic_string<CharT>` `s` consisting of the
abbreviated month name for the month represented by `m`, determined using the
locale associated with `os`, and inserts `s` into `os`.

Equivalent to

`return os << (m.ok() ? std::format(os.getloc(),
STATICALLY_WIDEN<CharT>("{:L%b}"), m) : std::format(os.getloc(),
STATICALLY_WIDEN<CharT>("{} is not a valid month"), unsigned(m)));`

where `STATICALLY_WIDEN<CharT>("...")` is `"..."` if `CharT` is `char`, and
`L"..."` if `CharT` is `wchar_t`.

### Return value

`os`

### Notes

This `operator<<` is primarily intended for debugging use. For control over
formatting, use `std::format`.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2372R3 | C++20 | the given locale was used by default | `L` is needed to use
      the given locale

### See also

- **format (C++20)** — stores formatted representation of the arguments in a new
  string (function template)
- **std::formatter<std::chrono::month> (C++20)** — formatting support for
  `month` (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/chrono/month/operator_ltlt*
