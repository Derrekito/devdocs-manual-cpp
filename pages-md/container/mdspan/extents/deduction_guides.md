# deduction guides for `std::extents`

```cpp
template< class... Integrals >
explicit extents( Integrals... ) -> /*see below*/;  // (since C++23)
```

A deduction guide is provided for `std::extents` to allow deduction from
integral arguments.

The deduced type is equivalent to `std::dextents<std::size_t,
sizeof...(Integrals)>`.

This overload participates in overload resolution only if
`(std::is_convertible_v<Integrals, std::size_t> && ...)` is true.

### Example

### See also

- **(constructor)** — constructs an `extents` (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/extents/deduction_guides*
