# std::movable

```cpp
template< class T >
concept movable =
    std::is_object_v<T> &&
    std::move_constructible<T> &&
    std::assignable_from<T&, T> &&
    std::swappable<T>;  // (since C++20)
```

The concept `movable<T>` specifies that `T` is an object type that can be moved
(that is, it can be move constructed, move assigned, and lvalues of type `T` can
be swapped).

### See also

- **copyable (C++20)** — specifies that an object of a type can be copied,
  moved, and swapped (concept)

---
*Source: https://en.cppreference.com/w/cpp/concepts/movable*
