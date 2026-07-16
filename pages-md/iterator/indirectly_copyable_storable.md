# std::indirectly_copyable_storable

```cpp
template< class In, class Out >
concept indirectly_copyable_storable =
    std::indirectly_copyable<In, Out> &&
    std::indirectly_writable<Out, std::iter_value_t<In>&> &&
    std::indirectly_writable<Out, const std::iter_value_t<In>&> &&
    std::indirectly_writable<Out, std::iter_value_t<In>&&> &&
    std::indirectly_writable<Out, const std::iter_value_t<In>&&> &&
    std::copyable<std::iter_value_t<In>> &&
    std::constructible_from<std::iter_value_t<In>, std::iter_reference_t<In>> &&
    std::assignable_from<std::iter_value_t<In>&, std::iter_reference_t<In>>;  // (since C++20)
```

The `indirectly_copyable_storable` concept specifies the relationship between an
`indirectly_readable` type and an `indirectly_writable` type. In addition to
`indirectly_copyable`, this concept specifies that the copy from the
`indirectly_readable` type can be performed via an intermediate object.

### Semantic requirements

`In` and `Out` model `std::indirectly_copyable_storable<In, Out>` only if given
a dereferenceable value `i` of type `In`:

- After the definition `std::iter_value_t<In> obj(*i);`, `obj` is equal to the
  value previously denoted by `*i`, and
- if `std::iter_reference_t<In>` is an rvalue reference type, `*i` is placed in
  a valid but unspecified state after the initialization of `obj`.

### Equality preservation

Expressions declared in requires expressions of the standard library concepts
are required to be equality-preserving (except where stated otherwise).

### See also

- **indirectly_copyable (C++20)** — specifies that values may be copied from an
  `indirectly_readable` type to an `indirectly_writable` type (concept)
- **indirectly_movable_storable (C++20)** — specifies that values may be moved
  from an `indirectly_readable` type to an `indirectly_writable` type and that
  the move may be performed via an intermediate object (concept)

---
*Source: https://en.cppreference.com/w/cpp/iterator/indirectly_copyable_storable*
