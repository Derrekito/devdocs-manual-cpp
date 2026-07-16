# std::allocation_result

```cpp
template< class Pointer, class SizeType = std::size_t >
struct allocation_result {
    Pointer ptr;
    SizeType count;
};  // (since C++23)
```

`allocation_result` specializations are returned from the `allocate_at_least`
member function of appropriate Allocator types (e.g.
`std::allocator::allocate_at_least`) and
`std::allocator_traits::allocate_at_least`.

Every specialization of `allocation_result` has no base classes or declared
members other than `ptr` and `count`, thus it is suitable for aggregate
initialization and structured binding.

### Template parameters

- **Pointer** — typically `std::allocator_traits<Alloc>::pointer`, where `Alloc`
  is an Allocator type
- **SizeType** — typically `std::allocator_traits<Alloc>::size_type`, where
  `Alloc` is an Allocator type

### Member objects

- **ptr (C++23)** — typically used for the address of the first element in the
  storage allocated by `allocate_at_least` (public member object)
- **count (C++23)** — typically used for the actual number of elements in the
  storage allocated by `allocate_at_least` (public member object)

### Notes

`Pointer` and `SizeType` are a pointer to an object type and
`std::make_unsigned_t<std::ptrdiff_t>` (which is almost always same as
`std::size_t`) by default.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_allocate_at_least` | 202302L | (C++23) | Size-feedback in the
      Allocator interface

### Example

### See also

- **allocate_at_least (C++23)** — allocates uninitialized storage at least as
  large as requested size (public member function of `std::allocator<T>`)
- **allocate_at_least [static] (C++23)** — allocates storage at least as large
  as the requested size via an allocator (public static member function of
  `std::allocator_traits<Alloc>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/allocation_result*
