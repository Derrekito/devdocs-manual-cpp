# std::extents<IndexType,Extents...>::*fwd-prod-of-extents*

```cpp
constexpr std::size_t rank_type /*fwd-prod-of-extents*/( rank_type i ) const noexcept;  // (since C++23) (exposition only*)
```

Returns the product of the sizes of extents with index less than `i`. The
behavior is undefined if `i <= rank()` is false.

### Parameters

- **i** — The end index of the range of extents to be multiplied together.

### Return value

If `i > 0` is true, return the product of `extent(k)` for all `k` in range
`[`​0​`,`i`)`, otherwise `1`.

### Example

### See also

- ***rev-prod-of-extents*** — returns the product of extents size in range `[`i
  + 1`,`rank()`)` (exposition-only member function*)

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/extents/fwd-prod-of-extents*
