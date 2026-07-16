# std::swap(std::basic_stringbuf)

```cpp
template< class CharT, class Traits, class Alloc >
void swap( std::basic_stringbuf<CharT,Traits,Alloc>& lhs,
           std::basic_stringbuf<CharT,Traits,Alloc>& rhs );  // (since C++11) (until C++20)
template< class CharT, class Traits, class Alloc >
void swap( std::basic_stringbuf<CharT,Traits,Alloc>& lhs,
           std::basic_stringbuf<CharT,Traits,Alloc>& rhs )
    noexcept(noexcept(lhs.swap(rhs)));  // (since C++20)
```

Overloads the `std::swap` algorithm for `std::basic_stringbuf`. Exchanges the
state of `lhs` with that of `rhs`. Effectively calls `lhs.swap(rhs)`.

### Parameters

- **lhs, rhs** — `std::basic_stringbuf` objects whose states to swap

### Return value

(none)

### Example

### See also

- **swap (C++11)** — swaps two `basic_stringbuf` objects (public member
  function)
- **swap** — swaps the values of two objects (function template)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_stringbuf/swap2*
