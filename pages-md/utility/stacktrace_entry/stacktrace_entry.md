# std::stacktrace_entry::stacktrace_entry

```cpp
constexpr stacktrace_entry() noexcept;  // (1) (since C++23)
constexpr stacktrace_entry( const stacktrace_entry& other ) noexcept;  // (2) (since C++23)
```

1) Default constructor. Creates an empty `stacktrace_entry`.

2) Copy constructor. Creates a copy of `other`.

### Parameters

- **other** — another `stacktrace_entry` to copy from

### Notes

A non-empty `stacktrace_entry` can be obtained from a `std::basic_stacktrace`
created by `std::basic_stacktrace::current` or a copy of such
`std::basic_stacktrace`.

### Example

### See also

- **(constructor)** — constructs a new `source_location` with
  implementation-defined values (public member function of
  `std::source_location`)

---
*Source: https://en.cppreference.com/w/cpp/utility/stacktrace_entry/stacktrace_entry*
