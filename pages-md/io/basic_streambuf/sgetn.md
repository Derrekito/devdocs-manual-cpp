# std::basic_streambuf<CharT,Traits>::sgetn, std::basic_streambuf<CharT,Traits>::xsgetn

```cpp
std::streamsize sgetn( char_type* s, std::streamsize count );  // (1)
protected:
virtual std::streamsize xsgetn( char_type* s, std::streamsize count );  // (2)
```

1) Calls `xsgetn(s, count)` of the most derived class.

2) Reads `count` characters from the input sequence and stores them into a
   character array pointed to by `s`. The characters are read as if by repeated
   calls to `sbumpc()`. That is, if less than `count` characters are immediately
   available, the function calls `uflow()` to provide more until `Traits::eof()`
   is returned.

Classes derived from `std::basic_streambuf` are permitted to provide more
   efficient implementations of this function.

### Parameters

- **s** — pointer to the beginning of a char_type array
- **count** — maximum number of characters to read.

### Return value

The number of characters successfully read. If it is less than `count` the input
sequence has reached the end.

### Notes

The rule about "more efficient implementations" permits bulk I/O without
intermediate buffering: that's how `std::ifstream::read` simply passes the
pointer to the POSIX `read()` system call in some implementations of iostreams

### Example

### See also

- **sputn** — invokes `xsputn()` (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/sgetn*
