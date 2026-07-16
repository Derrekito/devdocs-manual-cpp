# std::operator<<(std::error_code)

```cpp
template< class CharT, class Traits >
std::basic_ostream<CharT,Traits>&
    operator<<( basic_ostream<CharT,Traits>& os, const error_code& ec );  // (since C++11)
```

Performs stream output operation on error code `ec`.

Equivalent to `os << ec.category().name() << ':' << ec.value()`.

### Parameters

- **os** — output stream to insert data to
- **ec** — error code

### Return value

`os`

---
*Source: https://en.cppreference.com/w/cpp/error/error_code/operator_ltlt*
