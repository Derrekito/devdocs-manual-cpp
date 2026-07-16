# std::indirectly_copyable

```cpp
template< class In, class Out >
concept indirectly_copyable =
    std::indirectly_readable<In> &&
    std::indirectly_writable<Out, std::iter_reference_t<In>>;  // (since C++20)
```

The `indirectly_copyable` concept specifies the relationship between an
`indirectly_readable` type and a type that is `indirectly_writable`. The
`indirectly_writable` type must be able to directly copy the object that the
`indirectly_readable` type references.

### See also

- **indirectly_movable (C++20)** — specifies that values may be moved from an
  `indirectly_readable` type to an `indirectly_writable` type (concept)
- **indirectly_copyable_storable (C++20)** — specifies that values may be copied
  from an `indirectly_readable` type to an `indirectly_writable` type and that
  the copy may be performed via an intermediate object (concept)

---
*Source: https://en.cppreference.com/w/cpp/iterator/indirectly_copyable*
