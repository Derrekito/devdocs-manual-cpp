# std::swap(std::basic_syncbuf)

```cpp
template< class CharT, class Traits, class Allocator >
void swap( std::basic_syncbuf<CharT, Traits, Allocator>& lhs,
           std::basic_syncbuf<CharT, Traits, Allocator>& rhs );  // (since C++20)
```

Overloads the `std::swap` algorithm for `std::basic_syncbuf`. Exchanges the
state of `lhs` with that of `rhs`. Effectively calls `lhs.swap(rhs)`.

### Parameters

- **lhs, rhs** — `std::basic_syncbuf` objects whose states to swap

### Return value

(none)

### Example

### See also

- **swap** — swaps two `basic_syncbuf` objects (public member function)
- **swap** — swaps the values of two objects (function template)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_syncbuf/swap2*
