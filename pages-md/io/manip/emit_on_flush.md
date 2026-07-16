# std::emit_on_flush, std::noemit_on_flush

```cpp
template< class CharT, class Traits >
std::basic_ostream<CharT, Traits>& emit_on_flush( std::basic_ostream<CharT, Traits>& os );  // (1) (since C++20)
template< class CharT, class Traits >
std::basic_ostream<CharT, Traits>& noemit_on_flush( std::basic_ostream<CharT, Traits>& os );  // (2) (since C++20)
```

If `os.rdbuf()` actually points to a `std::basic_syncbuf<CharT, Traits,
Allocator>` `buf`, toggles whether it emits (i.e., transmits data to the
underlying stream buffer) when flushed:

1) calls `buf.set_emit_on_sync(true)`

2) calls `buf.set_emit_on_sync(false)`

Otherwise, these manipulators have no effect.

This is an output-only I/O manipulator, it may be called with an expression such
as `out << std::emit_on_flush` for any `out` of type `std::basic_ostream`.

### Parameters

- **os** — reference to output stream

### Return value

`os` (reference to the stream after manipulation)

### Example

### See also

- **set_emit_on_sync** — changes the current emit-on-sync policy (public member
  function of `std::basic_syncbuf<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/io/manip/emit_on_flush*
