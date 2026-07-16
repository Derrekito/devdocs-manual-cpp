# std::basic_filebuf<CharT,Traits>::uflow

```cpp
protected:
virtual int_type uflow()
```

Behaves like the `underflow()`, except that if `underflow()` succeeds (does not
return `Traits::eof()`), then advances the next pointer for the get area. In
other words, consumes one of the characters obtained by `underflow()`.

### Parameters

(none)

### Return value

The value of the character that was read and consumed in case of success, or
`Traits::eof()` in case of failure.

### Example

### See also

- **uflow [virtual]** — reads characters from the associated input sequence to
  the get area and advances the next pointer (virtual protected member function
  of `std::basic_streambuf<CharT,Traits>`)
- **underflow [virtual]** — reads from the associated file (virtual protected
  member function)
- **overflow [virtual]** — writes characters to the associated file from the put
  area (virtual protected member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_filebuf/uflow*
