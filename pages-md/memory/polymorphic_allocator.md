# std::pmr::polymorphic_allocator

```cpp
template< class T >
class polymorphic_allocator;  // (since C++17) (until C++20)
template< class T = std::byte >
class polymorphic_allocator;  // (since C++20)
```

The class template `std::pmr::polymorphic_allocator` is an Allocator which
exhibits different allocation behavior depending upon the
`std::pmr::memory_resource` from which it is constructed. Since
`memory_resource` uses runtime polymorphism to manage allocations, different
container instances with `polymorphic_allocator` as their static allocator type
are interoperable, but can behave as if they had different allocator types.

All specializations of `polymorphic_allocator` meet the allocator completeness
requirements.

The `polymorphic_allocator::construct` member function does uses-allocator
construction, so that the elements of a container using a
`polymorphic_allocator` will use that same allocator for their own allocations.
For example, a `std::pmr::vector<std::pmr::string>` will use the same
`memory_resource` for the `vector`'s storage and each `string`'s storage.

For non-polymorphic allocators, similar propagation can be achieved with the
help of `std::scoped_allocator_adaptor`.

### Member types

- **`value_type`** — `T`

### Member functions

- **(constructor)** — constructs a `polymorphic_allocator` (public member
  function)
- **(destructor) (implicitly declared)** — implicitly declared destructor
  (public member function)
- **operator= [deleted]** — copy assignment operator is deleted (public member
  function)

**Public member functions**

- **allocate** — allocate memory (public member function)
- **deallocate** — deallocate memory (public member function)
- **construct** — constructs an object in allocated storage (public member
  function)
- **destroy (deprecated in C++20)** — destroys an object in allocated storage
  (public member function)
- **allocate_bytes (C++20)** — allocate raw aligned memory from the underlying
  resource (public member function)
- **deallocate_bytes (C++20)** — free raw memory obtained from `allocate_bytes`
  (public member function)
- **allocate_object (C++20)** — allocates raw memory suitable for an object or
  an array (public member function)
- **deallocate_object (C++20)** — frees raw memory obtained by `allocate_object`
  (public member function)
- **new_object (C++20)** — allocates and constructs an object (public member
  function)
- **delete_object (C++20)** — destroys and deallocates an object (public member
  function)
- **select_on_container_copy_construction** — create a new
  `polymorphic_allocator` for use by a container's copy constructor (public
  member function)
- **resource** — returns a pointer to the underlying memory resource (public
  member function)

### Non-member functions

- **operator==operator!= (removed in C++20)** — compare two
  `polymorphic_allocator`s (function)

### Notes

`polymorphic_allocator` does not propagate on container copy assignment, move
assignment, or swap. As a result, move assignment of a
`polymorphic_allocator`-using container can throw, and swapping two
`polymorphic_allocator`-using containers whose allocators do not compare equal
results in undefined behavior.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_polymorphic_allocator` | 201902L | (C++20) |
      `std::pmr::polymorphic_allocator<>` as a vocabulary type

### See also

- **memory_resource (C++17)** — an abstract interface for classes that
  encapsulate memory resources (class)

---
*Source: https://en.cppreference.com/w/cpp/memory/polymorphic_allocator*
