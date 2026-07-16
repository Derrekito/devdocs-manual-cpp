# C++ named requirements: Destructible

Specifies that an instance of the type can be destructed.

### Requirements

The type `T` satisfies Destructible if

Given

- `u`, an expression of type `T`.

The following expressions must be valid and have their specified effects.

  Expression | Post-conditions
  `u.~T()` | All resources owned by `u` are reclaimed, no exceptions are thrown.

### Notes

Destructors are called implicitly at the end of object lifetime such as when
leaving scope or by the delete-expression. Explicit destructor call as shown in
the type requirement table is rare.

Thanks to pseudo destructor call, all scalar types meet the requirement of
Destructible, while array types and reference types do not. Note that
`std::is_destructible` allows arrays and reference types.

### See also

- **is_destructibleis_trivially_destructibleis_nothrow_destructible
  (C++11)(C++11)(C++11)** — checks if a type has a non-deleted destructor (class
  template)
- **destructible (C++20)** — specifies that an object of the type can be
  destroyed (concept)

---
*Source: https://en.cppreference.com/w/cpp/named_req/Destructible*
