# std::allocator<T>::allocate_at_least

```cpp
[[nodiscard]] constexpr std::allocation_result<T*, std::size_t>
    allocate_at_least( std::size_t n );  // (since C++23)
```

Allocates `count * sizeof(T)` bytes of uninitialized storage, where `count` is
an unspecified integer value not less than `n`, by calling `::operator new` (an
additional `std::align_val_t` argument might be provided), but it is unspecified
when and how this function is called.

Then, this function creates an array of type `T[count]` in the storage and
starts its lifetime, but does not start lifetime of any of its elements.

In order to use this function in a constant expression, the allocated storage
must be deallocated within the evaluation of the same expression.

Use of this function is ill-formed if `T` is an incomplete type.

### Parameters

- **n** — the lower bound of number of objects to allocate storage for

### Return value

`std::allocation_result<T*>{p, count}`, where `p` points to the first element of
an array of `count` objects of type `T` whose elements have not been constructed
yet.

### Exceptions

Throws `std::bad_array_new_length` if `std::numeric_limits<std::size_t>::max() /
sizeof(T) < n`, or `std::bad_alloc` if allocation fails.

### Notes

`allocate_at_least` is mainly provided for contiguous containers, e.g.
`std::vector` and `std::basic_string`, in order to reduce reallocation by making
their capacity match the actually allocated size when possible.

The "unspecified when and how" wording makes it possible to combine or optimize
away heap allocations made by the standard library containers, even though such
optimizations are disallowed for direct calls to `::operator new`. For example,
this is implemented by libc++ ([1] and [2]).

After calling `allocate_at_least` and before construction of elements, pointer
arithmetic of `T*` is well-defined within the allocated array, but the behavior
is undefined if elements are accessed.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_allocate_at_least` | 202302L | (C++23) | `allocate_at_least` etc.

### Example

### See also

- **allocation_result (C++23)** — records the address and the actual size of
  storage allocated by `allocate_at_least` (class template)
- **allocate_at_least [static] (C++23)** — allocates storage at least as large
  as the requested size via an allocator (public static member function of
  `std::allocator_traits<Alloc>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/allocator/allocate_at_least*
