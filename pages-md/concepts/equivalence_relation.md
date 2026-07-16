# std::equivalence_relation

```cpp
template< class R, class T, class U >
concept equivalence_relation = std::relation<R, T, U>;  // (since C++20)
```

The concept `equivalence_relation<R, T, U>` specifies that the `relation` `R`
imposes an equivalence relation on its arguments.

### Semantic requirements

A relation `r` is an equivalence relation if

- it is reflexive: for all `x`, `r(x, x)` is `true`;
- it is symmetric: for all `a` and `b`, `r(a, b)` is `true` if and only if `r(b,
  a)` is `true`;
- it is transitive: `r(a, b) && r(b, c)` implies `r(a, c)`.

### Notes

The distinction between `relation` and `equivalence_relation` is purely
semantic.

---
*Source: https://en.cppreference.com/w/cpp/concepts/equivalence_relation*
