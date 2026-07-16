# std::copy_constructible

```cpp
template< class T >
concept copy_constructible =
    std::move_constructible<T> &&
    std::constructible_from<T, T&> && std::convertible_to<T&, T> &&
    std::constructible_from<T, const T&> && std::convertible_to<const T&, T> &&
    std::constructible_from<T, const T> && std::convertible_to<const T, T>;  // (since C++20)
```

The concept `copy_constructible` is satisfied if `T` is an lvalue reference
type, or if it is a `move_constructible` object type where an object of that
type can constructed from a (possibly const) lvalue or const rvalue of that type
in both direct- and copy-initialization contexts with the usual semantics (a
copy is constructed with the source unchanged).

### Semantic requirements

If `T` is an object type, then `copy_constructible<T>` is modeled only if given

- `v`, an lvalue of type (possibly `const`) `T` or an rvalue of type `const T`,

the following are true:

- After the definition `T u = v;`, `u` is equal to `v` and `v` is not modified;
- `T(v)` is equal to `v` and does not modify `v`.

### See also

-
  **is_copy_constructibleis_trivially_copy_constructibleis_nothrow_copy_constructible
  (C++11)(C++11)(C++11)** — checks if a type has a copy constructor (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/concepts/copy_constructible*
