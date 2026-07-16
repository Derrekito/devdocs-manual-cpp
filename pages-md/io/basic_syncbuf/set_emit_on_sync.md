# std::basic_syncbuf<CharT,Traits,Allocator>::set_emit_on_sync

```cpp
void set_emit_on_sync( bool b ) noexcept;
```

Changes the current emit-on-sync policy.

The value `false` (the default) indicates that any flush will be postponed until
a call to emit.

The value `true` makes flushes apply immediately.

### Parameters

- **b** — new value for the emit-on-sync policy

### Example

### See also

- **sync [virtual]** — synchronizes the buffers with the associated character
  sequence (virtual protected member function of
  `std::basic_streambuf<CharT,Traits>`)
- **emit** — atomically transmits the entire internal buffer to the wrapped
  streambuf (public member function)
- **emit_on_flushnoemit_on_flush (C++20)** — controls whether a stream's
  `basic_syncbuf` emits on flush (function template)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_syncbuf/set_emit_on_sync*
