# std::relation

```cpp
template< class R, class T, class U >
concept relation =
    std::predicate<R, T, T> && std::predicate<R, U, U> &&
    std::predicate<R, T, U> && std::predicate<R, U, T>;  // (1) (since C++20)
```

The concept `relation<R, T, U>` specifies that `R` defines a binary relation
over the set of expressions whose type and value category are those encoded by
either `T` or `U`.

---
*Source: https://en.cppreference.com/w/cpp/concepts/relation*
