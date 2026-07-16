# std::basic_streambuf<CharT,Traits>::in_avail

```cpp
std::streamsize in_avail();
```

Returns the number of characters available in the get area. If a read position
is available, effectively returns `egptr() - gptr()`, the size of the get area.
In this case, the number of bytes returned is the number of bytes that can be
extracted from the buffer without calling `underflow()`.

If the get area is empty, calls `showmanyc()` to determine the number of bytes
available in the associated character sequence. In this case, the value returned
is the number of bytes that can be extracted from the buffer while it's
guaranteed that `underflow()` would not return `Traits::eof`.

### Parameters

(none)

### Return value

The number of characters available for non-blocking read (either the size of the
get area or the number of characters ready for reading from the associated
character sequence), or `-1` if no characters are available in the associated
sequence as far as `showmanyc()` can tell.

### Example

### See also

- **showmanyc [virtual]** — optionally provides the number of characters
  available for input from the file (virtual protected member function of
  `std::basic_filebuf<CharT,Traits>`)
- **readsome** — extracts already available blocks of characters (public member
  function of `std::basic_istream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/in_avail*
