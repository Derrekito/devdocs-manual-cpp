# std::destructible

```cpp
template< class T >
concept destructible = std::is_nothrow_destructible_v<T>;  // (since C++20)
```

The concept `destructible` specifies the concept of all types whose instances
can safely be destroyed at the end of their lifetime (including reference
types).

### Notes

Unlike the Destructible named requirement, `std::destructible` requires the
destructor to be `noexcept(true)`, not merely non-throwing when invoked, and
allows reference types and array types.

### See also

- **is_destructibleis_trivially_destructibleis_nothrow_destructible
  (C++11)(C++11)(C++11)** — checks if a type has a non-deleted destructor (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/concepts/destructible*
