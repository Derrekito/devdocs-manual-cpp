# std::pointer_traits<Ptr>::to_address

```cpp
static element_type* to_address( pointer p ) noexcept;  // (since C++20) (optional member of program-defined specialization)
```

Constructs a raw pointer that references the same object as its pointer-like
("fancy pointer") argument.

This function, if defined, is the inverse of `pointer_to`, and exists as the
customization point to be called by `std::to_address`.

### Parameters

- **p** — fancy pointer/pointer-like object

### Return value

A raw pointer of the type `element_type*` that references the same memory
location as the argument `p`.

### See also

- **pointer_to [static]** — obtains a dereferenceable pointer to its argument
  (public static member function)
- **to_address (C++20)** — obtains a raw pointer from a pointer-like type
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/pointer_traits/to_address*
