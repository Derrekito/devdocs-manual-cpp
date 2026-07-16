# std::swap(std::basic_ospanstream)

```cpp
template< class CharT, class Traits >
void swap( std::basic_ospanstream<CharT, Traits>& lhs,
           std::basic_ospanstream<CharT, Traits>& rhs );  // (since C++23)
```

Overloads the `std::swap` algorithm for `std::basic_ospanstream`. Exchanges the
state of `lhs` with that of `rhs`. Effectively calls `lhs.swap(rhs)`.

### Parameters

- **lhs, rhs** — streams whose state to swap

### Return value

(none)

### Exceptions

May throw implementation-defined exceptions.

### Example

### See also

- **swap (C++23)** — swaps two `basic_ospanstream` objects (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ospanstream/swap2*
