# operator==,!=(std::scoped_allocator_adaptor)

```cpp
template< class OuterAlloc1, class OuterAlloc2, class... InnerAllocs >
bool operator==( const scoped_allocator_adaptor<OuterAlloc1, InnerAllocs...>& lhs,
                 const scoped_allocator_adaptor<OuterAlloc2, InnerAllocs...>& rhs ) noexcept;  // (since C++11)
template< class OuterAlloc1, class OuterAlloc2, class... InnerAllocs >
bool operator!=( const scoped_allocator_adaptor<OuterAlloc1, InnerAllocs...>& lhs,
                 const scoped_allocator_adaptor<OuterAlloc2, InnerAllocs...>& rhs ) noexcept;  // (since C++11) (until C++20)
```

Compares two scoped allocator adaptors. Two such allocators are equal if:

- `lhs.outer_allocator() == rhs.outer_allocator()`, and
- if `sizeof...(InnerAllocs) > 0`, `lhs.inner_allocator() ==
  rhs.inner_allocator()`.

The `!=` operator is synthesized from `operator==`.
*(since C++20)*

### Parameters

- **lhs, rhs** — scoped allocator adaptors to compare

### Return value

1) Returns `true` if `lhs` and `rhs` are equal, `false` otherwise.

2) Returns `true` if `lhs` and `rhs` are not equal, `false` otherwise.

---
*Source: https://en.cppreference.com/w/cpp/memory/scoped_allocator_adaptor/operator_cmp*
