# std::swap(std::basic_spanbuf)

```cpp
template< class CharT, class Traits >
void swap( std::basic_spanbuf<CharT, Traits>& lhs,
           std::basic_spanbuf<CharT, Traits>& rhs );  // (since C++23)
```

Overloads the `std::swap` algorithm for `std::basic_spanbuf`. Exchanges the
state of `lhs` with that of `rhs`. Equivalent to `lhs.swap(rhs);`.

### Parameters

- **lhs, rhs** — `std::basic_spanbuf` objects whose states to swap

### Return value

(none)

### Exceptions

May throw implementation-defined exceptions.

### Example

### See also

- **swap (C++23)** — swaps two `basic_spanbuf` objects (public member function)
- **swap** — swaps the values of two objects (function template)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_spanbuf/swap2*
