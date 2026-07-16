# std::swap(std::basic_ispanstream)

```cpp
template< class CharT, class Traits >
void swap( std::basic_ispanstream<CharT, Traits>& lhs,
           std::basic_ispanstream<CharT, Traits>& rhs );  // (since C++23)
```

Overloads the `std::swap` algorithm for `std::basic_ispanstream`. Exchanges the
state of `lhs` with that of `rhs`. Effectively calls `lhs.swap(rhs)`.

### Parameters

- **lhs, rhs** — streams whose state to swap

### Return value

(none)

### Exceptions

May throw implementation-defined exceptions.

### Example

### See also

- **swap (C++23)** — swaps two `basic_ispanstream` objects (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ispanstream/swap2*
