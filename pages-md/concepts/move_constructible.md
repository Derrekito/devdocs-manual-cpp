# std::move_constructible

```cpp
template< class T >
concept move_constructible = std::constructible_from<T, T> && std::convertible_to<T, T>;  // (since C++20)
```

The concept `move_constructible` is satisfied if `T` is a reference type, or if
it is an object type where an object of that type can be constructed from an
rvalue of that type in both direct- and copy-initialization contexts, with the
usual semantics.

### Semantic requirements

If `T` is an object type, then `move_constructible<T>` is modeled only if given

- `rv`, an rvalue of type `T`, and
- `u2`, a distinct object of type `T` equal to `rv`,

the following are true:

- After the definition `T u = rv;`, `u` is equal to `u2`;
- `T(rv)` is equal to `u2`; and
- If `T` is not const-qualified, then `rv`'s resulting state (after the
  definition/expression is evaluated in either bullets above) is valid but
  unspecified; otherwise, it is unchanged.

### See also

-
  **is_move_constructibleis_trivially_move_constructibleis_nothrow_move_constructible
  (C++11)(C++11)(C++11)** — checks if a type can be constructed from an rvalue
  reference (class template)

---
*Source: https://en.cppreference.com/w/cpp/concepts/move_constructible*
