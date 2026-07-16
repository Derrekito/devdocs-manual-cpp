# std::get(std::complex)

```cpp
template< std::size_t I >
friend constexpr T& get( std::complex<T>& x );  // (1) (since C++26)
template< std::size_t I >
friend constexpr const T& get( const std::complex<T>& x );  // (2) (since C++26)
template< std::size_t I >
friend constexpr T&& get( std::complex<T>&& x );  // (3) (since C++26)
template< std::size_t I >
friend constexpr const T&& get( const std::complex<T>&& x );  // (4) (since C++26)
```

Returns the reference to real or imaginary part from a `complex` when `I == 0`
or `I == 1`, respectively. It is mainly provided for structured binding support.

### Parameters

- **x** — a `complex`

### Return value

1-4) A reference to the real or imaginary part from the stored one when `I == 0`
   or `I == 1`, respectively.

### Example

### See also

- **Structured binding (C++17)** — binds the specified names to sub-objects or
  tuple elements of the initializer
- **get(std::tuple) (C++11)** — tuple accesses specified element (function
  template)
- **get(std::pair) (C++11)** — accesses an element of a `pair` (function
  template)
- **get(std::array) (C++11)** — accesses an element of an `array` (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/get*
