# std::type_index::operator==,!=,<,<=,>,>=,<=>

```cpp
bool operator==( const type_index& rhs ) const noexcept;  // (1) (since C++11)
bool operator!=( const type_index& rhs ) const noexcept;  // (2) (since C++11) (until C++20)
bool operator<( const type_index& rhs ) const noexcept;  // (3) (since C++11)
bool operator<=( const type_index& rhs ) const noexcept;  // (4) (since C++11)
bool operator>( const type_index& rhs ) const noexcept;  // (5) (since C++11)
bool operator>=( const type_index& rhs ) const noexcept;  // (6) (since C++11)
std::strong_ordering operator<=>( const type_index& rhs ) const noexcept;  // (7) (since C++20)
```

Compares the underlying `std::type_info` objects.

1,2) Checks whether the underlying `std::type_info` objects refer to the same
   type.

3-7) Compares the underlying `std::type_info` objects as defined by an
   implementation-defined ordering. The comparison is done by
   `type_info::before`.

The `!=` operator is synthesized from `operator==`.
*(since C++20)*

### Parameters

- **rhs** — another `type_index` object to compare to

### Return value

1) `true` if the underlying `std::type_info` objects refer to the same type,
   `false` otherwise.

2) `true` if the underlying `std::type_info` objects refer not to the same type,
   `false` otherwise.

3-6) `true` if the types referred by the underlying `std::type_info` objects are
   ordered by corresponding order, `false` otherwise.

7) `std::strong_ordering::equal` if the underlying `std::type_info` objects
   refer to the same type, otherwise `std::strong_ordering::less` if `*this`'s
   underlying `std::type_info` object precedes `rhs`'s in the
   implementation-defined ordering, otherwise `std::strong_ordering::greater`.

---
*Source: https://en.cppreference.com/w/cpp/types/type_index/operator_cmp*
