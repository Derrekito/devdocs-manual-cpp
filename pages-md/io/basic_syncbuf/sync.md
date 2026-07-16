# std::basic_syncbuf<CharT,Traits,Allocator>::sync

```cpp
protected:
int sync() override;
```

First, records that a flush is pending, by updating the appropriate private
flag.

Then, if the current emit-on-sync policy is `true`, makes a call to `emit()`.

Otherwise (if the emit-on-sync policy is `false`, which is the default), the
flush is suspended until `emit()` is called, such as through
`std::basic_osyncstream::emit()` or `std::basic_osyncstream::~basic_osyncstream`

### Parameters

(none)

### Notes

`sync()` or its equivalent is implicitly called by `close()`, `seekoff()`, and
`seekpos()` and explicitly called by `std::basic_streambuf::pubsync()`

### Example

### See also

- **sync [virtual]** — synchronizes the buffers with the associated character
  sequence (virtual protected member function of
  `std::basic_streambuf<CharT,Traits>`)
- **emit** — calls `emit()` on the underlying `basic_syncbuf` to transmit its
  internal data to the final destination (public member function of
  `std::basic_osyncstream<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_syncbuf/sync*
