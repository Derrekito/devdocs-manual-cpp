# std::basic_istringstream::swap

```cpp
void swap( basic_istringstream& other );  // (since C++11)
```

Exchanges the state of the stream with those of `other`.

This is done by calling `basic_istream<CharT, Traits>::swap(other)` and
`rdbuf()->swap(*other.rdbuf())`.

### Parameters

- **other** — stream to exchange the state with

### Return value

(none)

### Exceptions

May throw implementation-defined exceptions.

### Example

### See also

- **operator= (C++11)** — moves the string stream (public member function)
- **swap (C++11)** — swaps two `basic_stringbuf` objects (public member function
  of `std::basic_stringbuf<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istringstream/swap*
