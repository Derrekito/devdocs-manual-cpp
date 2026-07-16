# std::mdspan<T,Extents,LayoutPolicy,AccessorPolicy>::mdspan

```cpp
constexpr mdspan();  // (1) (since C++23)
template< class... OtherIndexTypes >
    constexpr explicit mdspan( data_handle_type p, OtherIndexTypes... exts );  // (2) (since C++23)
template< class OtherIndexType, std::size_t N >
    constexpr explicit(N != rank_dynamic())
        mdspan( data_handle_type p, std::span<OtherIndexType, N> exts );  // (3) (since C++23)
template< class OtherIndexType, std::size_t N >
    constexpr explicit(N != rank_dynamic())
        mdspan( data_handle_type p, const std::array<OtherIndexType, N>& exts );  // (4) (since C++23)
constexpr mdspan( data_handle_type p, const extents_type& ext );  // (5) (since C++23)
constexpr mdspan( data_handle_type p, const mapping_type& m );  // (6) (since C++23)
constexpr mdspan( data_handle_type p, const mapping_type& m,
                  const accessor_type& a );  // (7) (since C++23)
template< class OtherElementType, class OtherExtents,
          class OtherLayoutPolicy, class OtherAccessor >
    constexpr explicit(/* see below */)
        mdspan( const mdspan<OtherElementType, OtherExtents,
                             OtherLayoutPolicy, OtherAccessor>& other );  // (8) (since C++23)
constexpr mdspan( const mdspan& rhs ) = default;  // (9) (since C++23)
constexpr mdspan( mdspan&& rhs ) = default;  // (10) (since C++23)
```

Construct an `mdspan`, optionally using user-supplied data handle `p`, layout
mapping `m`, and accessor `a`. If extents `exts` or `ext` are supplied, they are
converted to `extents_type` and used to initialize the layout mapping.

1) Construct an empty mdspan. Value-initializes `ptr_`, `map_`, and `acc_`.

- The behavior is undefined if `[`​0​`,`map_.required_span_size()`)` is not an
  accessible range of `ptr_` and `acc_` for the values of `map_` and `acc_`
  after the invocation of this constructor.
- This overload participates in overload resolution only if

2) Construct an mdspan over the underlying data referred by `p` with extents
represented by `exts...`. Value-initializes `acc_`, direct-non-list-initializes
`ptr_` with `std::move(p)` and `map_` with
`extents_type(static_cast<index_type>(std::move(exts))...)`.

- The behavior is undefined if `[`​0​`,`map_.required_span_size()`)` is not an
  accessible range of `ptr_` and `acc_` for the values of `map_` and `acc_`
  after the invocation of this constructor.
- Let `N` be `sizeof...(OtherIndexTypes)`. This overload participates in
  overload resolution only if

3,4) Construct an mdspan over the underlying data referred by `p` with extents
represented by pack `exts`. Value-initializes `acc_`,
direct-non-list-initializes `ptr_` with `std::move(p)` and `map_` with
`extents_type(exts)`.

- The behavior is undefined if `[`​0​`,`map_.required_span_size()`)` is not an
  accessible range of `ptr_` and `acc_` for the values of `map_` and `acc_`
  after the invocation of this constructor.
- This overload participates in overload resolution only if

5) Construct an mdspan over the underlying data referred by `p` with extents
represented by `ext`. Value-initializes `acc_`, direct-non-list-initializes
`ptr_` with `std::move(p)` and `map_` with `exts`.

- The behavior is undefined if `[`​0​`,`map_.required_span_size()`)` is not an
  accessible range of `p` and `acc_` for the values of `map_` and `acc_` after
  the invocation of this constructor.
- This overload participates in overload resolution only if

6) Construct an mdspan over the underlying data referred by `p` with layout
mapping `m`. Value-initializes `acc_`, direct-non-list-initializes `ptr_` with
`std::move(p)` and `map_` with `m`.

- The behavior is undefined if `[`​0​`,`m.required_span_size()`)` is not an
  accessible range of `p` and `acc_` for the values of `acc_` after the
  invocation of this constructor.
- This overload participates in overload resolution only if
  `std::is_default_constructible_v<accessor_type>` is `true`.

7) Construct an mdspan over the underlying data referred by `p` with layout
mapping `m` and accessor `a`. Direct-non-list-initializes `ptr_` with
`std::move(p)`, `map_` with `m` and `acc_` with `a`.

- The behavior is undefined if `[`​0​`,`m.required_span_size()`)` is not an
  accessible range of `p` and `a` after the invocation of this constructor.

8) Converting constructor from another mdspan. Direct-non-list-initializes
`ptr_` with `other.ptr_`, `map_` with `other.map_` and `acc_` with `other.acc_`.

- The behavior is undefined if :
- This overload participates in overload resolution only if
- The program is ill-formed if:
- The expression inside explicit is equivalent to: `!std::is_convertible_v<const
  OtherLayoutPolicy:: template mapping<OtherExtents>&, mapping_type>||
  !std::is_convertible_v<const OtherAccessor&, accessor_type>`

9) Defaulted copy constructor.

10) Defaulted move constructor.

### Parameters

- **p** — a handle to the underlying data
- **m** — a layout mapping
- **a** — an accessor
- **ext** — a `std::extents` object
- **exts** — represents a multi-dimensional extents
- **other** — another mdspan to convert from
- **rhs** — another mdspan to copy or move from

### Example

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/mdspan*
