# std::stacktrace_entry::source_line

```cpp
std::uint_least32_t source_line() const;  // (since C++23)
```

Returns a 1-based line number that lexically relates to the evaluation
represented by `*this`, or 0 on failure other than allocation failure, e.g. when
`*this` is empty.

Either `source_file` returns the presumed source file name and `source_line`
returns the presumed line number, or `source_file` returns the actual source
file name and `source_line` returns the actual line number.

### Parameters

(none)

### Return value

The line number specified above on success, 0 on failure other than allocation
failure.

### Exceptions

Throws `std::bad_alloc` if memory for the internal data structures cannot be
allocated.

### Notes

The presumed line number is what the predefined macro `__LINE__` expands to, and
can be changed by the `#line` directive.

This function is not required to be `noexcept` because getting source line
requires allocation on some platforms.

### Example

### See also

- **line** — return the line number represented by this object (public member
  function of `std::source_location`)

---
*Source: https://en.cppreference.com/w/cpp/utility/stacktrace_entry/source_line*
