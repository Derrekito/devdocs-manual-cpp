# std::basic_ispanstream<CharT,Traits>::operator=

```cpp
basic_ispanstream& operator=( basic_ispanstream&& other );  // (1) (since C++23)
basic_ispanstream& operator=( const basic_ispanstream& ) = delete;  // (2) (since C++23)
```

1) Move assigns the `other` to `*this`, effectively move-assigning both the
   `std::basic_istream` base class subobject and the wrapped
   `std::basic_spanbuf`.

2) Copy assignment operator is deleted. `basic_ispanstream` is not copy
   assignable.

Note that move assignment operator of the base class swaps all stream state
variables (except for `rdbuf()`) between `*this` and `other`.

It is implementation-defined whether the `std::basic_spanbuf` wrapped in `other`
still holds an underlying buffer after the move assignment.

### Parameters

- **other** — another stream to move from

### Return value

`*this`

### Exceptions

May throw implementation-defined exceptions.

### Example

### See also

- **operator= (C++23)** — assigns a `basic_spanbuf` object (public member
  function of `std::basic_spanbuf<CharT,Traits>`)
- **operator= (C++11)** — move-assigns from another `basic_istream` (protected
  member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ispanstream/operator%3D*
