# std::tuple_element<std::complex>

```cpp
template< std::size_t I, class T >
struct tuple_element<I, std::complex<T>>;  // (since C++26)
```

The partial specializations of `std::tuple_element` for `std::complex` provide
compile-time access to the underlying real and imaginary number type of a
`complex`, using tuple-like syntax. They are provided for structured binding
support. The program is ill-formed if `I` >= 2.

### Member types

- **`type`** — `T`

### Example

### See also

- **Structured binding (C++17)** — binds the specified names to sub-objects or
  tuple elements of the initializer
- **tuple_element (C++11)** — obtains the element types of a tuple-like type
  (class template)
- **std::tuple_size<std::complex> (C++26)** — obtains the number of components
  of a `std::complex` (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/tuple_element*
