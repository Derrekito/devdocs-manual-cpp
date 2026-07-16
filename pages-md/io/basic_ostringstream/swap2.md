# std::swap(std::basic_ostringstream)

```cpp
template< class CharT, class Traits, class Alloc >
void swap( std::basic_ostringstream<CharT,Traits,Alloc>& lhs,
           std::basic_ostringstream<CharT,Traits,Alloc>& rhs );  // (since C++11)
```

Overloads the `std::swap` algorithm for `std::basic_ostringstream`. Exchanges
the state of `lhs` with that of `rhs`. Effectively calls `lhs.swap(rhs)`.

### Parameters

- **lhs, rhs** — streams whose state to swap

### Return value

(none)

### Exceptions

May throw implementation-defined exceptions.

### Example

### See also

- **swap (C++11)** — swaps two string streams (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ostringstream/swap2*
