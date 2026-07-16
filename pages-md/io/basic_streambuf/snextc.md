# std::basic_streambuf<CharT,Traits>::snextc

```cpp
int_type snextc();
```

Advances the input sequence by one character and reads one character.

The function calls `sbumpc()` to advance the input sequence. If that function
returns `Traits::eof()` meaning that input sequence has been exhausted and
`uflow()` could not retrieve more data, `Traits::eof()` is returned. Otherwise
`sgetc()` is called in order to read the character.

### Parameters

(none)

### Return value

The value of the next character. If the input sequence has been exhausted,
`Traits::eof()` is returned.

### Example

### See also

- **sgetc** — reads one character from the input sequence without advancing the
  sequence (public member function)
- **sbumpcstossc (removed in C++17)** — reads one character from the input
  sequence and advances the sequence (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/snextc*
