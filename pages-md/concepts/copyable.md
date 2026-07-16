# std::copyable

```cpp
template< class T >
concept copyable =
    std::copy_constructible<T> &&
    std::movable<T> &&
    std::assignable_from<T&, T&> &&
    std::assignable_from<T&, const T&> &&
    std::assignable_from<T&, const T>;  // (since C++20)
```

The concept `copyable<T>` specifies that `T` is a `movable` object type that can
also be copied (that is, it supports copy construction and copy assignment).

### See also

- **movable (C++20)** — specifies that an object of a type can be moved and
  swapped (concept)

---
*Source: https://en.cppreference.com/w/cpp/concepts/copyable*
