# std::swappable, std::swappable_with

```cpp
template< class T >
concept swappable =
    requires(T& a, T& b) {
        ranges::swap(a, b);
    };  // (1) (since C++20)
template< class T, class U >
concept swappable_with =
    std::common_reference_with<T, U> &&
    requires(T&& t, U&& u) {
        ranges::swap(std::forward<T>(t), std::forward<T>(t));
        ranges::swap(std::forward<U>(u), std::forward<U>(u));
        ranges::swap(std::forward<T>(t), std::forward<U>(u));
        ranges::swap(std::forward<U>(u), std::forward<T>(t));
    };  // (2) (since C++20)
```

The concept `swappable<T>` specifies that lvalues of type `T` are swappable.

The concept `swappable_with<T, U>` specifies that expressions of the type and
value category encoded by `T` and `U` are swappable with each other.
`swappable_with<T, U>` is satisfied only if a call to `ranges::swap(t, u)`
exchanges the value of `t` and `u`, that is, given distinct objects `t2` equal
to `t` and `u2` equal to `u`, after evaluating either `ranges::swap(t, u)` or
`ranges::swap(u, t)`, `t2` is equal to `u` and `u2` is equal to `t`.

### Equality preservation

Expressions declared in requires expressions of the standard library concepts
are required to be equality-preserving (except where stated otherwise).

---
*Source: https://en.cppreference.com/w/cpp/concepts/swappable*
