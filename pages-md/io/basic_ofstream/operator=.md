# std::basic_ofstream<CharT,Traits>::operator=

```cpp
basic_ofstream& operator=( basic_ofstream&& other );  // (since C++11)
```

Move assigns the file stream `other` to `*this`, effectively move-assigning both
the `std::basic_ostream` base class and the associated `std::basic_filebuf`.

`other` is left with no associated file. Note that the base class move
assignment swaps all stream state variables (except for `rdbuf`) between `*this`
and `other`.

### Parameters

- **other** — file stream to move

### Return value

`*this`

### Example

### See also

- **swap (C++11)** — swaps two file streams (public member function)
- **operator= (C++11)** — assigns a `basic_filebuf` object (public member
  function of `std::basic_filebuf<CharT,Traits>`)
- **operator= (C++11)** — move-assigns from another `basic_ostream` (protected
  member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ofstream/operator%3D*
