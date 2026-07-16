# std::basic_spanbuf<CharT,Traits>::operator=

```cpp
basic_spanbuf& operator=( basic_spanbuf&& rhs );  // (1) (since C++23)
basic_spanbuf& operator( const basic_spanbuf& ) = delete;  // (2) (since C++23)
```

1) Move assignment operator. Equivalent to `auto tmp{std::move(rhs)};
   this->swap(tmp); return *this;`. After move assignment, `*this` holds the
   state `rhs` held before move assignment. It is implementation-defined whether
   `rhs` still hold the underlying buffer after move assignment.

2) The copy assignment operator is deleted; `basic_spanbuf` is not
   CopyAssignable.

### Parameters

- **rhs** — another `basic_spanbuf` that will be moved from

### Return value

`*this`

### Exceptions

May throw implementation-defined exceptions.

### Example

### See also

- **(constructor) (C++23)** — constructs a `basic_spanbuf` object (public member
  function)
- **swap (C++23)** — swaps two `basic_spanbuf` objects (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_spanbuf/operator%3D*
