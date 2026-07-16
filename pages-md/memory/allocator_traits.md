# std::allocator_traits

```cpp
template< class Alloc >
struct allocator_traits;  // (since C++11)
```

The `allocator_traits` class template provides the standardized way to access
various properties of Allocators. The standard containers and other standard
library components access allocators through this template, which makes it
possible to use any class type as an allocator, as long as the user-provided
specialization of `std::allocator_traits` implements all required functionality.

A program that declares an explicit or partial specialization of
`std::allocator_traits` is ill-formed, no diagnostic required.
*(since C++23)*

The default, non-specialized, `std::allocator_traits` contains the following
members:

### Member types

- **`allocator_type`** — `Alloc`
- **`value_type`** — `Alloc::value_type`
- **`pointer`** — `Alloc::pointer` if present, otherwise `value_type*`
- **`const_pointer`** — `Alloc::const_pointer` if present, otherwise
  std::pointer_traits<pointer>::rebind<const value_type>
- **`void_pointer`** — `Alloc::void_pointer` if present, otherwise
  std::pointer_traits<pointer>::rebind<void>
- **`const_void_pointer`** — `Alloc::const_void_pointer` if present, otherwise
  std::pointer_traits<pointer>::rebind<const void>
- **`difference_type`** — `Alloc::difference_type` if present, otherwise
  std::pointer_traits<pointer>::difference_type
- **`size_type`** — `Alloc::size_type` if present, otherwise
  std::make_unsigned<difference_type>::type
- **`propagate_on_container_copy_assignment`** —
  `Alloc::propagate_on_container_copy_assignment` if present, otherwise
  `std::false_type`
- **`propagate_on_container_move_assignment`** —
  `Alloc::propagate_on_container_move_assignment` if present, otherwise
  `std::false_type`
- **`propagate_on_container_swap`** — `Alloc::propagate_on_container_swap` if
  present, otherwise `std::false_type`
- **`is_always_equal`** — `Alloc::is_always_equal` if present, otherwise
  std::is_empty<Alloc>::type

### Member alias templates

- **`rebind_alloc<T>`** — `Alloc::rebind<T>::other` if present, otherwise
  `SomeAllocator<T, Args>` if this `Alloc` is of the form SomeAllocator<U,
  Args>, where Args is zero or more type arguments
- **`rebind_traits<T>`** — std::allocator_traits<rebind_alloc<T>>

### Member functions

- **allocate [static]** — allocates uninitialized storage using the allocator
  (public static member function)
- **allocate_at_least [static] (C++23)** — allocates storage at least as large
  as the requested size via an allocator (public static member function)
- **deallocate [static]** — deallocates storage using the allocator (public
  static member function)
- **construct [static]** — constructs an object in the allocated storage
  (function template)
- **destroy [static]** — destructs an object stored in the allocated storage
  (function template)
- **max_size [static]** — returns the maximum object size supported by the
  allocator (public static member function)
- **select_on_container_copy_construction [static]** — obtains the allocator to
  use after copying a standard container (public static member function)

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2108 | C++11 | there was no way to show an allocator is stateless |
      `is_always_equal` provided

### See also

- **allocator** — the default allocator (class template)
- **scoped_allocator_adaptor (C++11)** — implements multi-level allocator for
  multi-level containers (class template)
- **pointer_traits (C++11)** — provides information about pointer-like types
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/memory/allocator_traits*
