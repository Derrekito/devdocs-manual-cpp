# std::basic_stringbuf<CharT,Traits,Allocator>::*init_buf_ptrs*

```cpp
void init_buf_ptrs();  // (exposition only*)
```

Initializes the input and output sequences from `buf` according to `mode`. `buf`
and `mode` are exposition-only data members of `*this`.

Immediately after this function returns:

- If `std::ios_base::out` is set in `mode`, `pbase()` points to `buf.front()`
  and `epptr() >= pbase() + buf.size()` is `true`; in addition, if
  `std::ios_base::ate` is set in `mode`, `pptr() == pbase() + buf.size()` is
  `true`, otherwise `pptr() == pbase()` is `true`.
- If `std::ios_base::in` is set in `mode`, `eback()` points to `buf.front()`,
  and `gptr() == eback() && egptr() == eback() + buf.size()` is `true`.

### Notes

For efficiency reasons, stream buffer operations can violate invariants of `buf`
while it is held encapsulated in the `std::basic_stringbuf`, e.g., by writing to
characters in the range `[``buf.data() + buf.size()``,``buf.data() +
buf.capacity()``)`.

All operations retrieving a `std::basic_string` from `buf` ensure that the
`std::basic_string` invariants hold on the returned value.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 1448 | C++98 | calling `init_buf_ptrs()` made `pptr() == pbase() +
      buf.data()` for streams that are both input and output streams | makes
      `pptr() == pbase() + buf.size()` for append streams

---
*Source: https://en.cppreference.com/w/cpp/io/basic_stringbuf/init_buf_ptrs*
