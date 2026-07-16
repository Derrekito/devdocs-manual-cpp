# std::basic_streambuf<CharT,Traits>::sbumpc, std::basic_streambuf<CharT,Traits>::stossc

```cpp
int_type sbumpc();  // (1)
void stossc();  // (2) (deprecated in C++98) (removed in C++17)
```

Reads one character and advances the input sequence by one character.

1) If the input sequence read position is not available, returns `uflow()`.
   Otherwise returns `Traits::to_int_type(*gptr())`.

2) Same as (1), but discards the result.

### Parameters

(none)

### Return value

1) The value of the character pointed to by the *get pointer*, or
   `Traits::eof()` if the read position is not available.

### Example

### See also

- **sgetc** — reads one character from the input sequence without advancing the
  sequence (public member function)
- **snextc** — advances the input sequence, then reads one character without
  advancing again (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/sbumpc*
