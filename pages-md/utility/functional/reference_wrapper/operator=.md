# std::reference_wrapper<T>::operator=

```cpp
reference_wrapper& operator=( const reference_wrapper& other ) noexcept;  // (since C++11) (until C++20)
constexpr reference_wrapper& operator=( const reference_wrapper& other ) noexcept;  // (since C++20)
```

Copy assignment operator. Drops the current reference and stores a reference to
`other.get()`.

### Parameters

- **other** — reference wrapper to copy

### Return value

`*this`

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/reference_wrapper/operator%3D*
