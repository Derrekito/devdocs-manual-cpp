# std::basic_spanbuf<CharT,Traits>::span

```cpp
std::span<CharT> span() const noexcept;  // (1) (since C++23)
void span( std::span<CharT> s ) noexcept;  // (2) (since C++23)
```

1) Gets a `span` referencing the written area if `std::ios_base::out` is set in
   the open mode, or a `span` referencing the underlying buffer otherwise.

2) Makes the `basic_spanbuf` perform I/O on the buffer referenced by `s`. Sets
   pointers to get area, put area, or both. Set bits in open mode (affecting
   pointers to get area) Return value after setting `eback()` `gptr()` `egptr()`
   `std::ios_base::in` `s.data()` `s.data()` `s.data() + s.size()` Set bits in
   open mode (affecting pointers to put area) Return value after setting
   `pbase()` `pptr()` `epptr()` `std::ios_base::out && !std::ios_base::ate`
   `s.data()` `s.data()` `s.data() + s.size()` `std::ios_base::out &&
   std::ios_base::ate` `s.data()` `s.data() + s.size()` `s.data() + s.size()`

### Parameters

- **s** — a `std::span` that references the user-provided buffer

### Return value

1) `std::span<CharT>(pbase(), pptr())` if `std::ios_base::out` is set in the
   open mode, or a `std::span<CharT>` that references the whole underlying
   buffer otherwise.

2) (none)

### Example

### See also

- **str** — replaces or obtains a copy of the associated character string
  (public member function of `std::basic_stringbuf<CharT,Traits,Allocator>`)
- **view (C++20)** — obtains a view over the underlying character sequence
  (public member function of `std::basic_stringbuf<CharT,Traits,Allocator>`)
- **str** — marks the buffer frozen and returns the beginning pointer of the
  input sequence (public member function of `std::strstreambuf`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_spanbuf/span*
