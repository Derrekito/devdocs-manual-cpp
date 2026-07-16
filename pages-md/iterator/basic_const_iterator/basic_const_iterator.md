# std::basic_const_iterator<Iter>::basic_const_iterator

```cpp
basic_const_iterator() requires std::default_initializable<Iter> = default;  // (1) (since C++23)
constexpr basic_const_iterator( Iter x );  // (2) (since C++23)
template< std::convertible_to<Iter> U >
constexpr basic_const_iterator( basic_const_iterator<U> other );  // (3) (since C++23)
template< /*different-from*/<basic_const_iterator> T >
    requires std::convertible_to<T, Iter>
constexpr basic_const_iterator( T&& x );  // (4) (since C++23)
```

Constructs a new iterator adaptor.

1) Default constructor. The underlying iterator is value-initialized.

2) The underlying iterator is initialized with `std::move(x)`.

3) The underlying iterator is initialized with that of `other`.

4) The underlying iterator is initialized with `std::forward<T>(x)`.

### Parameters

- **x** — iterator to adapt
- **other** — iterator adaptor to copy

### Example

### See also

- **operator=** — assigns another iterator adaptor (public member function)

---
*Source: https://en.cppreference.com/w/cpp/iterator/basic_const_iterator/basic_const_iterator*
