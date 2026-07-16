# operator-(std::move_iterator)

```cpp
template< class Iterator1, class Iterator2 >
auto operator-( const move_iterator<Iterator1>& lhs,
                const move_iterator<Iterator2>& rhs
              ) -> decltype(lhs.base() - rhs.base());  // (since C++11) (until C++17)
template< class Iterator1, class Iterator2 >
constexpr auto operator-( const move_iterator<Iterator1>& lhs,
                          const move_iterator<Iterator2>& rhs
                        ) -> decltype(lhs.base() - rhs.base());  // (since C++17)
```

Returns the distance between two iterator adaptors.

### Parameters

- **lhs, rhs** — iterator adaptors to compute the difference of

### Return value

`lhs.base() - rhs.base()`

### Example

### See also

-
  **operator++operator++(int)operator+=operator+operator--operator--(int)operator-=operator-
  (C++11)** — advances or decrements the iterator (public member function)
- **operator+ (C++11)** — advances the iterator (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/move_iterator/operator-*
