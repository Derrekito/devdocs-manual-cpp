# std::move_sentinel<S>::move_sentinel

```cpp
constexpr move_sentinel();  // (1) (since C++20)
constexpr explicit move_sentinel( S x );  // (2) (since C++20)
template< class S2 >
    requires std::convertible_to<const S2&, S>
        constexpr move_sentinel( const std::move_sentinel<S2>& other );  // (3) (since C++20)
```

Constructs a new sentinel adaptor.

1) Default constructor. The underlying sentinel is value-initialized.

2) The underlying sentinel is initialized with `x`.

3) The underlying sentinel is initialized with that of `other`.

### Parameters

- **x** — sentinel to adapt
- **other** — sentinel adaptor to copy

### Example

### See also

- **(constructor) (C++11)** — constructs a new iterator adaptor (public member
  function of `std::move_iterator<Iter>`)

---
*Source: https://en.cppreference.com/w/cpp/iterator/move_sentinel/move_sentinel*
