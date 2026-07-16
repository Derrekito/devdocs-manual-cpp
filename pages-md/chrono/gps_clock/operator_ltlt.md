# std::chrono::operator<<(std::chrono::gps_time)

```cpp
template< class CharT, class Traits, class Duration >
std::basic_ostream<CharT, Traits>&
    operator<<( std::basic_ostream<CharT, Traits>& os,
                const std::chrono::gps_time<Duration>& tp );  // (since C++20)
```

Outputs `tp` into the stream `os`, as if by `os << std::format(os.getloc(),
STATICALLY-WIDEN("{:L%F %T}"), tp)`, where `STATICALLY_WIDEN<CharT>("{:L%F
%T}")` is `"{:L%F %T}"` if `CharT` is `char`, and `L"{:L%F %T}"` if `CharT` is
`wchar_t`.

### Return value

`os`

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2372R3 | C++20 | the given locale was used by default | `L` is needed to use
      the given locale

### See also

- **std::formatter<std::chrono::gps_time> (C++20)** — formatting support for
  `gps_time` (class template specialization)
- **format (C++20)** — stores formatted representation of the arguments in a new
  string (function template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/gps_clock/operator_ltlt*
