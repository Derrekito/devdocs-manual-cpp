# std::chrono::operator<<(std::chrono::local_time)

```cpp
template< class CharT, class Traits, class Duration >
std::basic_ostream<CharT, Traits>&
    operator<<( std::basic_ostream<CharT, Traits>& os,
                const std::chrono::local_time<Duration>& tp );  // (since C++20)
```

Outputs `tp` into the stream `os`, as if by `os <<
std::chrono::sys_time<Duration>(tp.time_since_epoch());`.

### Return value

`os`

### See also

- **operator<<(std::chrono::sys_time) (C++20)** — performs stream output on a
  `sys_time` (function template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/local_t/operator_ltlt*
