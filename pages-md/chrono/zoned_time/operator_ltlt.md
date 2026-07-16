# std::chrono::operator<<(std::chrono::zoned_time)

```cpp
template< class CharT, class Traits, class Duration, class TimeZonePtr >
std::basic_ostream<CharT, Traits>&
    operator<<( std::basic_ostream<CharT, Traits>& os,
                const std::chrono::zoned_time<Duration, TimeZonePtr>& tp );  // (since C++20)
```

Outputs `tp` to the stream `os`, as if by `std::format(os.getloc(), fmt, tp)`,
where `fmt` is `"{:L%F %T %Z}"` if `CharT` is `char`, or `L"{:L%F %T %Z}"` if
`CharT` is `wchar_t`.

### Parameters

- **os** — output stream
- **tp** — `zoned_time` to output

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

- **std::formatter<std::chrono::zoned_time> (C++20)** — formatting support for
  `zoned_time` (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/chrono/zoned_time/operator_ltlt*
