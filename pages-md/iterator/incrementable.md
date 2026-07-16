# std::incrementable

```cpp
template< class I >
    concept incrementable =
        std::regular<I> &&
        std::weakly_incrementable<I> &&
        requires(I i) {
            { i++ } -> std::same_as<I>;
        };  // (since C++20)
```

This concept specifies requirements on types that can be incremented with the
pre- and post-increment operators, whose increment operations are
equality-preserving, and the type is `std::equality_comparable`.

Unlike with `std::weakly_incrementable`, which only support single-pass
algorithms, multi-pass one-directional algorithms can be used with types that
model `std::incrementable`.

### Semantic requirements

`I` models `std::incrementable` only if given any two incrementable objects `a`
and `b` of type `I`:

- `bool(a == b)` implies `bool(a++ == b)`, and
- `bool(a == b)` implies `bool(((void)a++, a) == ++b)`.

### Equality preservation

Expressions declared in requires expressions of the standard library concepts
are required to be equality-preserving (except where stated otherwise).

### See also

- **weakly_incrementable (C++20)** — specifies that a `semiregular` type can be
  incremented with pre- and post-increment operators (concept)
- **same_as (C++20)** — specifies that a type is the same as another type
  (concept)

---
*Source: https://en.cppreference.com/w/cpp/iterator/incrementable*
