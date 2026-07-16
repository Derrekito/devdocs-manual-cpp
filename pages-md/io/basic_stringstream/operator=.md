# std::basic_stringstream::operator=

```cpp
basic_stringstream& operator=( basic_stringstream&& other );  // (since C++11)
```

Move assigns the string stream `other` to `*this`, effectively move-assigning
both the `std::basic_iostream` base class and the associated
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
- **operator= (C++11)** — move-assigns another `basic_iostream` (protected
  member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_stringstream/operator%3D*
