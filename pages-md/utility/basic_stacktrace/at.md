# std::basic_stacktrace<Allocator>::at

```cpp
const_reference at( size_type pos ) const;  // (since C++23)
```

Returns a reference to the entry at specified location `pos`, with bounds
checking.

If `pos` is not within the range of the stacktrace, an exception of type
`std::out_of_range` is thrown.

### Parameters

- **pos** — position of the stacktrace entry to return

### Return value

Reference to the requested entry.

### Exceptions

`std::out_of_range` if `pos >= size()`.

### Complexity

Constant.

### Example

### See also

- **operator[]** — access specified stacktrace entry (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/basic_stacktrace/at*
