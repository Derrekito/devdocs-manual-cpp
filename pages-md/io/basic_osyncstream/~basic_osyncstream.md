# std::basic_osyncstream<CharT,Traits,Allocator>::~basic_osyncstream

```cpp
~basic_osyncstream();
```

Destroys a synchronized output stream.

The destruction of the member `std::basic_syncbuf` will emit any buffered output
not yet emitted.

### Parameters

(none)

### Example

### See also

- **(destructor)** — destroys the `basic_syncbuf` and emits its internal buffer
  (public member function of `std::basic_syncbuf<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_osyncstream/%7Ebasic_osyncstream*
