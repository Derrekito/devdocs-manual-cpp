# std::indirectly_movable

```cpp
template< class In, class Out >
concept indirectly_movable =
    std::indirectly_readable<In> &&
    std::indirectly_writable<Out, std::iter_rvalue_reference_t<In>>;  // (since C++20)
```

The `indirectly_movable` concept specifies the relationship between an
`indirectly_readable` type and a type that is `indirectly_writable`. The
`indirectly_writable` type must be able to directly move the object that the
`indirectly_readable` type references.

### See also

- **indirectly_movable_storable (C++20)** — specifies that values may be moved
  from an `indirectly_readable` type to an `indirectly_writable` type and that
  the move may be performed via an intermediate object (concept)
- **indirectly_copyable (C++20)** — specifies that values may be copied from an
  `indirectly_readable` type to an `indirectly_writable` type (concept)

---
*Source: https://en.cppreference.com/w/cpp/iterator/indirectly_movable*
