# std::basic_ispanstream<CharT,Traits>::swap

```cpp
void swap( basic_ispanstream& other );  // (since C++23)
```

Exchanges the state of the stream with those of `other`.

This is done by calling `std::basic_istream<CharT, Traits>::swap(other)` and
swapping the wrapped `std::basic_spanbuf` objects (accessible through
`*rdbuf()`).

### Parameters

- **other** — stream to exchange the state with

### Return value

(none)

### Exceptions

May throw implementation-defined exceptions.

### Example

### See also

- **swap (C++23)** — swaps two `basic_spanbuf` objects (public member function
  of `std::basic_spanbuf<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ispanstream/swap*
