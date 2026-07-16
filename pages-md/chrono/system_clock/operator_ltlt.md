# std::chrono::operator<<(std::chrono::sys_time)

```cpp
template< class CharT, class Traits, class Duration >
std::basic_ostream<CharT, Traits>&
    operator<<( std::basic_ostream<CharT, Traits>& os,
                const std::chrono::sys_time<Duration>& tp );  // (1) (since C++20)
template< class CharT, class Traits, class Duration >
std::basic_ostream<CharT, Traits>&
    operator<<( std::basic_ostream<CharT, Traits>& os,
                const std::chrono::sys_days& tp );  // (2) (since C++20)
```

Outputs `tp` into the stream `os`.

1) Equivalent to: return os << std::format(os.getloc(),
   STATICALLY-WIDEN<CharT>("{:L%F %T}"), tp); where
   `STATICALLY_WIDEN<CharT>("{:L%F %T}")` is `"{:L%F %T}"` if `CharT` is `char`,
   and `L"{:L%F %T}"` if `CharT` is `wchar_t`. This overload participates in
   overload resolution only if `std::chrono::treat_as_floating_point_v<typename
   Duration::rep>` is `false` and `Duration(1) < std::chrono::days(1)`.

2) Equivalent to `os << std::chrono::year_month_day(tp);`.

### Return value

`os`

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2372R3 | C++20 | the given locale was used by default | `L` is needed to use
      the given locale

### See also

- **std::formatter<std::chrono::sys_time> (C++20)** — formatting support for
  `sys_time` (class template specialization)
- **format (C++20)** — stores formatted representation of the arguments in a new
  string (function template)
- **operator<< (C++20)** — outputs a `year_month_day` into a stream (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/system_clock/operator_ltlt*
