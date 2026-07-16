# std::error_category::operator==,!=,<,<=>

```cpp
bool operator==( const error_category& rhs ) const noexcept;  // (1) (since C++11)
bool operator!=( const error_category& rhs ) const noexcept;  // (2) (since C++11) (until C++20)
bool operator<( const error_category& rhs ) const noexcept;  // (3) (since C++11) (until C++20)
std::strong_ordering operator<=>( const error_category& rhs ) const noexcept;  // (4) (since C++20)
```

Compares to another error category.

1) Checks whether `*this` and `rhs` refer to the same object.

2) Checks whether `*this` and `rhs` do not refer to the same object.

3) Orders `*this` and `rhs` by the order of `this` and `&rhs`. Equivalent to
   `std::less<const error_category*>()(this, &rhs)`.

4) Orders `*this` and `rhs` by the order of `this` and `&rhs`. Equivalent to
   `std::compare_three_way()(this, &rhs)`.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.
*(since C++20)*

### Parameters

- **rhs** — specifies the `error_category` to compare

### Return value

1) `true` if `*this` and `rhs` refer to the same object, `false` otherwise.

2) `true` if `*this` and `rhs` do not refer to the same object, `false`
   otherwise.

3) `true` if `*this` is less than `rhs` as defined by the order of `this` and
   `&rhs`.

4) `std::strong_order::less` if `*this` is less than `rhs` as defined by the
   order of `this` and `&rhs`, otherwise `std::strong_order::greater` if `rhs`
   is less than `*this` in the order, otherwise `std::strong_order::equal`.

---
*Source: https://en.cppreference.com/w/cpp/error/error_category/operator_cmp*
