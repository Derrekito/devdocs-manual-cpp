# std::stacktrace_entry::description

```cpp
std::string description() const;  // (since C++23)
```

Returns a description of the evaluation represented by `*this` on success, or an
empty string on failure other than allocation failure, e.g. when `*this` is
empty.

### Parameters

(none)

### Return value

A description of the represented evaluation on success, an empty string on
failure other than allocation failure.

### Exceptions

Throws `std::bad_alloc` if memory for the internal data structures or the
resulting string cannot be allocated.

### Notes

Custom allocators support for this function is not provided, because the
implementations usually require platform specific allocations, system calls and
a lot of CPU intensive work, while a custom allocator does not provide benefits
for this function as the platform specific operations take an order of magnitude
more time than the allocation.

### Example

---
*Source: https://en.cppreference.com/w/cpp/utility/stacktrace_entry/description*
