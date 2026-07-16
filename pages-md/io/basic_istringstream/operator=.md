# std::basic_istringstream::operator=

```cpp
basic_istringstream& operator=( basic_istringstream&& other );  // (since C++11)
```

Move assigns the string stream `other` to `*this`, effectively move-assigning
both the `std::basic_istream` base class and the associated
`std::basic_stringbuf`.

Note that the base class move assignment swaps all stream state variables
(except for `rdbuf`) between `*this` and `other`.

### Parameters

- **other** — string stream to move from

### Return value

`*this`

### Example

### See also

- **swap (C++11)** — swaps two string streams (public member function)
- **operator= (C++11)** — assigns a `basic_stringbuf` object (public member
  function of `std::basic_stringbuf<CharT,Traits,Allocator>`)
- **operator= (C++11)** — move-assigns from another `basic_istream` (protected
  member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istringstream/operator%3D*
