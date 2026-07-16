# std::swap(std::basic_regex)

```cpp
template< class CharT, class Traits >
void swap( basic_regex<CharT, Traits>& lhs, basic_regex<CharT, Traits>& rhs ) noexcept;  // (since C++11)
```

Overloads the `std::swap` algorithm for `std::basic_regex`. Exchanges the state
of `lhs` with that of `rhs`. Effectively calls `lhs.swap(rhs)`.

### Parameters

- **lhs, rhs** — regular expressions to swap

### Return value

(none)

### Example

### See also

- **swap** — swaps the contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/regex/basic_regex/swap2*
