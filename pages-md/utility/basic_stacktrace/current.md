# std::basic_stacktrace<Allocator>::current

```cpp
static basic_stacktrace current( const allocator_type& alloc =
                                     allocator_type() ) noexcept;  // (1) (since C++23)
static basic_stacktrace current( size_type skip, const allocator_type& alloc =
                                     allocator_type() ) noexcept;  // (2) (since C++23)
static basic_stacktrace current( size_type skip, size_type max_depth,
                                 const allocator_type& alloc =
                                     allocator_type() ) noexcept;  // (3) (since C++23)
```

Let `s[i]` (0 ≤ `i` < `n`) denote the (`i+1`)-th stacktrace entry in the
stacktrace of the current evaluation in the current thread of execution, where
`n` is the count of the stacktrace entries in the stackentry.

1) Attempts to create a `basic_stacktrace` consisting of `s[0]`, `s[1]`, ...,
   `s[n - 1]`.

2) Attempts to create a `basic_stacktrace` consisting of `s[m]`, `s[m + 1]`,
   ..., `s[n - 1]`, where `m` is `min(skip, n)`.

3) Attempts to create a `basic_stacktrace` consisting of `s[m]`, `s[m + 1]`,
   ..., `s[o - 1]`, where `m` is `min(skip, n)` and `o` is `min(skip +
   max_depth, n)`. The behavior is undefined if the `skip + max_depth < skip`
   (i.e. the mathematical result of `skip + max_depth` overflows).

In all cases, `alloc` is stored into the created `basic_stacktrace` and used to
allocate the storage for stacktrace entries.

### Parameters

- **alloc** — allocator to use for all memory allocations of the constructed
  `basic_stacktrace`
- **skip** — the number of stacktrace entries to skip
- **max_depth** — the maximum depth of the stacktrace entries

### Return value

If the allocation succeeds, the `basic_stacktrace` described above.

Otherwise, an empty `basic_stacktrace`.

### Example

### See also

- **(constructor)** — creates a new `basic_stacktrace` (public member function)
- **current [static]** — constructs a new `source_location` corresponding to the
  location of the call site (public static member function of
  `std::source_location`)

---
*Source: https://en.cppreference.com/w/cpp/utility/basic_stacktrace/current*
