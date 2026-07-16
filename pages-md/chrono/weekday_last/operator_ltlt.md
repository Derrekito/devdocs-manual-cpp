# std::chrono::operator<<(std::chrono::weekday_last)

```cpp
template< class CharT, class Traits >
std::basic_ostream<CharT, Traits>&
    operator<<( std::basic_ostream<CharT, Traits>& os, const std::chrono::weekday_last& wdl );  // (since C++20)
```

Outputs a textual representation of `wdl` into the stream `os`, as if by

`os << std::format(os.getloc(), STATICALLY_WIDEN<CharT>("{:L}[last]"),
wdl.weekday());`

where `STATICALLY_WIDEN<CharT>("{:L}[last]")` is `"{:L}[last]"` if `CharT` is
`char`, and `L"{:L}[last]"` if `CharT` is `wchar_t`.

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
*Source: https://en.cppreference.com/w/cpp/chrono/weekday_last/operator_ltlt*
