# std::ostreambuf_iterator<CharT,Traits>::failed

```cpp
bool failed() const throw();  // (until C++11)
bool failed() const noexcept;  // (since C++11)
```

Returns `true` if the iterator encountered the end-of-file condition, that is,
if an earlier call to `std::basic_streambuf::sputc` (made by `operator=`)
returned `Traits::eof`.

### Parameters

(none)

### Return value

`true` if this iterator has encountered the end-of-file condition on output,
`false` otherwise.

### Example

---
*Source: https://en.cppreference.com/w/cpp/iterator/ostreambuf_iterator/failed*
