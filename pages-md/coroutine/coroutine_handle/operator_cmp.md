# operator==, operator<=>(std::coroutine_handle)

```cpp
constexpr bool
    operator==( std::coroutine_handle<> x, std::coroutine_handle<> y ) noexcept;  // (1) (since C++20)
constexpr std::strong_ordering
    operator<=>( std::coroutine_handle<> x, std::coroutine_handle<> y ) noexcept;  // (2) (since C++20)
```

Compares two `std::coroutine_handle<>` values `x` and `y` according to their
underlying addresses.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.

### Parameters

- **x, y** — `std::coroutine_handle<>` values to compare

### Return value

1) `x.address() == y.address()`

2) `std::compare_three_way{}(x.address(), y.address())`

### Notes

Although these operators are only overloaded for `std::coroutine_handle<>`,
other specializations of `std::coroutine_handle` are also equality comparable
and three-way comparable, because they are implicitly convertible to
`std::coroutine_handle<>`.

### Example

---
*Source: https://en.cppreference.com/w/cpp/coroutine/coroutine_handle/operator_cmp*
