# std::mdspan<T,Extents,LayoutPolicy,AccessorPolicy>::operator[]

```cpp
template< class... OtherIndexTypes >
constexpr reference operator[]( OtherIndexTypes... indices ) const;  // (1) (since C++23)
template< class OtherIndexType >
constexpr reference operator[]( std::span<OtherIndexType, rank()> indices ) const;  // (2) (since C++23)
template< class OtherIndexType >
constexpr reference operator[]( const std::array<OtherIndexType, rank()>& indices ) const;  // (3) (since C++23)
```

Returns a reference to the `indices`th element of the mdspan.

1) Equivalent to `return acc_.access(ptr_,
map_(static_cast<index_type>(std::move(indices))...));`.

- This overload participates in overload resolution only if:
- Let `I` be `extents_type::index-cast(std::move(indices))`. Then the behavior
  is undefined if `I` is not a multidimensional index in `extents()`, i.e.,
  `map_(I) < map_.required_span_size()` is `false`.

2,3) Let `P` be a parameter pack such that
`std::is_same_v<make_index_sequence<rank()>, index_sequence<P...>>` is `true`,
then the operator is equivalent to `return
operator[](std::as_const(indices[P])...);`.

- This overload participates in overload resolution only if:

### Parameters

- **indices** — the indices of the element to access

### Return value

A reference to the element.

### Example

### See also

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/operator_at*
