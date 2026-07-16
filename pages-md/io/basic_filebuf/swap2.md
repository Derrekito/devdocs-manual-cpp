# std::swap(std::basic_filebuf)

```cpp
template< class CharT, class Traits >
void swap( std::basic_filebuf<CharT,Traits>& lhs,
           std::basic_filebuf<CharT,Traits>& rhs );  // (since C++11)
```

Overloads the `std::swap` algorithm for `std::basic_filebuf`. Exchanges the
state of `lhs` with that of `rhs`. Effectively calls `lhs.swap(rhs)`.

### Parameters

- **lhs, rhs** — `std::basic_filebuf` objects whose states to swap

### Return value

(none)

### Example

### See also

- **swap (C++11)** — swaps two `basic_filebuf` objects (public member function)
- **swap** — swaps the values of two objects (function template)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_filebuf/swap2*
