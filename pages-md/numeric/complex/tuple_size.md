# std::tuple_size<std::complex>

```cpp
template< class T >
struct tuple_size<std::complex<T>>
    : std::integral_constant<std::size_t, 2> {};  // (since C++26)
```

The partial specialization of `std::tuple_size` for `std::complex` provides a
compile-time way to obtain the number of components of a `complex`, which is
always 2, using tuple-like syntax. It is provided for structured binding
support.

## Inherited from std::integral_constant

### Member constants

- **value [static]** — the constant value 2 (public static member constant)

### Member functions

- **operator std::size_t** — converts the object to std::size_t, returns `value`
  (public member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — std::size_t
- **`type`** — std::integral_constant<std::size_t, value>

### Example

```cpp
#include <complex>

static_assert(std::tuple_size_v<std::complex<long long>> == 2);

int main() {}
```

### See also

- **Structured binding (C++17)** — binds the specified names to sub-objects or
  tuple elements of the initializer
- **tuple_size (C++11)** — obtains the number of elements of a tuple-like type
  (class template)
- **std::tuple_element<std::complex> (C++26)** — obtains the underlying real and
  imaginary number type of a `std::complex` (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/tuple_size*
