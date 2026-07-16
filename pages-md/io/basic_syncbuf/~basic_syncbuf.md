# std::basic_syncbuf<CharT,Traits,Allocator>::~basic_syncbuf

```cpp
~basic_syncbuf();
```

Calls `emit()` to transmit all pending output (and delayed flush, if any) to the
wrapped stream. If an exception is thrown by this call, it is caught and
ignored.

### Parameters

(none)

### Notes

Normally called by the destructor of the owning `std::basic_osyncstream`.

### Example

### See also

- **(destructor)** — destroys the `basic_osyncstream` and emits its internal
  buffer (public member function of
  `std::basic_osyncstream<CharT,Traits,Allocator>`)
- **(constructor)** — constructs a `basic_syncbuf` object (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_syncbuf/%7Ebasic_syncbuf*
