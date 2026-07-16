# *voidify*

```cpp
template< class T >
constexpr void* voidify( T& obj ) noexcept; // exposition only  // (since C++20)
```

Returns the address of `obj` (implicitly converted to void*).

### Parameters

- **obj** — the object whose address will be taken

### Return value

`std::addressof(obj)`

### Notes

This exposition-only function is introduced by P0896R4. It is used to describe
the effects of uninitialized memory algorithms which construct objects in
uninitialized memory areas. The result pointer is used as the placement-params
of a placement new expression.

Initially, the return value was `const_cast<void*>(static_cast<const volatile
void*>(std::addressof(obj)))`, which breaks const-correctness. The explicit
casts were removed by the resolution of LWG issue 3870, and the only conversion
left is the implicit conversion to void*.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3870 | C++20 | the explicit casts broke const-correctness | removed these
      casts

### See also

- **uninitialized_copy** — copies a range of objects to an uninitialized area of
  memory (function template)
- **uninitialized_fill** — copies an object to an uninitialized area of memory,
  defined by a range (function template)
- **uninitialized_move (C++17)** — moves a range of objects to an uninitialized
  area of memory (function template)
- **uninitialized_default_construct (C++17)** — constructs objects by
  default-initialization in an uninitialized area of memory, defined by a range
  (function template)
- **uninitialized_value_construct (C++17)** — constructs objects by
  value-initialization in an uninitialized area of memory, defined by a range
  (function template)
- **construct_at (C++20)** — creates an object at a given address (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/memory/voidify*
