# std::stacktrace_entry::source_file

```cpp
std::string source_file() const;  // (since C++23)
```

Returns the presumed or actual name of the source file that lexically contains
the expression or statement whose evaluation is represented by `*this`, or an
empty string on failure other than allocation failure, e.g. when `*this` is
empty.

Either `source_file` returns the presumed source file name and `source_line`
returns the presumed line number, or `source_file` returns the actual source
file name and `source_line` returns the actual line number.

### Parameters

(none)

### Return value

The name of the source file specified above on success, an empty string on
failure other than allocation failure.

### Exceptions

Throws `std::bad_alloc` if memory for the internal data structures or the
resulting string cannot be allocated.

### Notes

The presumed name of the source file is what the predefined macro `__FILE__`
expands to, and can be changed by the `#line` directive.

Custom allocators support for this function is not provided, because the
implementations usually require platform specific allocations, system calls and
a lot of CPU intensive work, while a custom allocator does not provide benefits
for this function as the platform specific operations take an order of magnitude
more time than the allocation.

### Example

### See also

- **file_name** — return the file name represented by this object (public member
  function of `std::source_location`)

---
*Source: https://en.cppreference.com/w/cpp/utility/stacktrace_entry/source_file*
