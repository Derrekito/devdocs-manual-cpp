# std::move_sentinel<S>::operator=

```cpp
template< class S2 >
    requires std::assignable_from<S&, const S2&>
        constexpr move_sentinel& operator=( const std::move_sentinel<S2>& other );  // (since C++20)
```

The underlying sentinel is assigned the value of the underlying sentinel of
`other`, i.e. `other.base()`.

### Parameters

- **other** — sentinel adaptor to assign

### Return value

`*this`

### Example

### See also

- **operator= (C++11)** — assigns another iterator adaptor (public member
  function of `std::move_iterator<Iter>`)

---
*Source: https://en.cppreference.com/w/cpp/iterator/move_sentinel/operator%3D*
