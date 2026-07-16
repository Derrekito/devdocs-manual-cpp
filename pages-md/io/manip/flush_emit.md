# std::flush_emit

```cpp
template< class CharT, class Traits >
std::basic_ostream<CharT, Traits>& flush_emit( std::basic_ostream<CharT, Traits>& os );  // (since C++20)
```

Flushes the output sequence `os` as if by calling `os.flush()`. Then, if
`os.rdbuf()` actually points to a `std::basic_syncbuf<CharT, Traits, Allocator>`
`buf`, calls `buf.emit()`.

This is an output-only I/O manipulator, it may be called with an expression such
as `out << std::flush_emit` for any `out` of type `std::basic_ostream`.

### Parameters

- **os** — reference to output stream

### Return value

`os` (reference to the stream after manipulation)

### Example

### See also

- **flush** — synchronizes with the underlying storage device (public member
  function of `std::basic_ostream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/manip/flush_emit*
