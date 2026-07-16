# std::pmr::memory_resource

```cpp
class memory_resource;  // (since C++17)
```

The class `std::pmr::memory_resource` is an abstract interface to an unbounded
set of classes encapsulating memory resources.

### Member functions

- **(constructor) (implicitly declared)** — constructs a new `memory_resource`
  (public member function)
- **(destructor) [virtual]** — destructs a `memory_resource` (virtual public
  member function)
- **operator= (implicitly declared)** — Implicitly declared copy assignment
  operator (public member function)

**Public member functions**

- **allocate** — allocates memory (public member function)
- **deallocate** — deallocates memory (public member function)
- **is_equal** — compare for equality with another `memory_resource` (public
  member function)

**Private member functions**

- **do_allocate [virtual]** — allocates memory (virtual private member function)
- **do_deallocate [virtual]** — deallocates memory (virtual private member
  function)
- **do_is_equal [virtual]** — compare for equality with another
  `memory_resource` (virtual private member function)

### Non-member-functions

- **operator==operator!= (removed in C++20)** — compare two `memory_resource`s
  (function)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_memory_resource` | 201603L | (C++17) | `std::pmr::memory_resource`

---
*Source: https://en.cppreference.com/w/cpp/memory/memory_resource*
