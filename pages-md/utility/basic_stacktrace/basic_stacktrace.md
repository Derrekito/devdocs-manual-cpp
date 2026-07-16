# std::basic_stacktrace<Allocator>::basic_stacktrace

```cpp
basic_stacktrace() noexcept(/* see below */);  // (1) (since C++23)
explicit basic_stacktrace( const allocator_type& alloc ) noexcept;  // (2) (since C++23)
basic_stacktrace( const basic_stacktrace& other );  // (3) (since C++23)
basic_stacktrace( basic_stacktrace&& other ) noexcept;  // (4) (since C++23)
basic_stacktrace( const basic_stacktrace& other,
                  const allocator_type& alloc );  // (5) (since C++23)
basic_stacktrace( basic_stacktrace&& other, const allocator_type& alloc );  // (6) (since C++23)
```

Constructs an empty `basic_stacktrace`, or copy/move from `other`.

1) Default constructor. Constructs an empty `basic_stacktrace` with a
   default-constructed allocator.

2) Constructs an empty `basic_stacktrace` using `alloc` as the allocator.

3) Copy constructor. Constructs a `basic_stacktrace` with the copy of the
   contents of `other`, the allocator is obtained as if by calling
   `std::allocator_traits<allocator_type>::select_on_container_copy_construction(other.get_allocator())`.

4) Move constructor. Constructs a `basic_stacktrace` with the contents of
   `other` using move semantics. Allocator is move-constructed from that of
   `other`. After construction, `other` is left in a valid but unspecified
   state.

5) Same as the copy constructor, except that `alloc` is used as the allocator.

6) Behaves same as the move constructor if `alloc == other.get_allocator()`.
   Otherwise, allocates memory with `alloc` and performs element-wise move.
   `alloc` is used as the allocator.

(3,5,6) may throw an exception or construct an empty `basic_stacktrace` on
allocation failure.

### Parameters

- **alloc** — allocator to use for all memory allocations of the constructed
  `basic_stacktrace`
- **other** — another `basic_stacktrace` to copy/move from

### Exceptions

1) `noexcept` specification:
   `noexcept(std::is_nothrow_default_constructible_v<allocator_type>)`

3,5,6) May propagate the exception thrown on allocation failure.

### Complexity

1,2) Constant.

3) Linear in size of `other`.

4) Constant.

5) Linear in size of `other`.

6) Linear in size of `other` if `alloc != other.get_allocator()`, otherwise
   constant.

### Notes

After container move construction (overload (4)), references, pointers, and
iterators (other than the end iterator) to `other` remain valid, but refer to
elements that are now in `*this`. The current standard makes this guarantee via
the blanket statement in [container.reqmts]/67, and a more direct guarantee is
under consideration via LWG issue 2321.

### Example

### See also

- **operator=** — assigns to the `basic_stacktrace` (public member function)
- **current [static]** — obtains the current stacktrace or its given part
  (public static member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/basic_stacktrace/basic_stacktrace*
