# std::extents<IndexType,Extents...>::extents

```cpp
constexpr extents() = default;  // (1) (since C++23)
template< class OtherIndexType, std::size_t... OtherExtents >
constexpr explicit(/*see below*/)
    extents( const std::extents<OtherIndexType, OtherExtents...>& other ) noexcept;  // (2) (since C++23)
template< class... OtherIndexTypes >
constexpr explicit extents( OtherIndexTypes... exts ) noexcept;  // (3) (since C++23)
template< class OtherIndexType, std::size_t N >
constexpr explicit(N != rank_dynamic())
    extents( std::span<OtherIndexType, N> exts ) noexcept;  // (4) (since C++23)
template< class OtherIndexType, std::size_t N >
constexpr explicit(N != rank_dynamic())
    extents( const std::array<OtherIndexType, N>& exts ) noexcept;  // (5) (since C++23)
```

Construct an `extents`. One can construct `extents` from just dynamic extents,
which are all the values getting stored, or from all the extents with a
precondition.

1) Default constructor. Initializes all dynamic extents to zero.
2) Conversion from another `extents` object. After construction, `*this ==
other` is true.

- The behavior is undefined if
- This overload participates in overload resolution only if
- This constructor is explicit if

3) Let `N` be `sizeof...(exts)` and `exts_arr` be `std::array<IndexType,
N>{static_cast<IndexType>(std::move(exts))...}`, equivalent to
`extents(exts_arr)`.

- This overload participates in overload resolution only if
- The behavior is undefined if

4,5) If `N` equals `rank_dynamic()`, for all `d` in
`[``​0​``,``rank_dynamic()``)`, direct-non-list-initializes `dynamic-extents[d]`
with `std::as_const(exts[d])`. Otherwise, for all `d` in
`[``​0​``,``rank_dynamic()``)`, direct-non-list-initializes `dynamic-extents[d]`
with `std::as_const(exts[dynamic-index-inv(d)])`.

- This overload participates in overload resolution only if
- The behavior is undefined if

### Parameters

- **other** — another `extents` to convert from
- **exts** — represents the extents

### Example

### See also

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/extents/extents*
