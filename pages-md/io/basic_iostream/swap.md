# std::basic_iostream<CharT,Traits>::swap

```cpp
protected:
void swap( basic_iostream& other );  // (since C++11)
```

Exchanges the state with another input/output stream object. Effectively calls
`basic_istream<CharT,Traits>::swap(other)`.

This member function is protected: it is called by the swap member functions of
the derived stream classes `std::basic_stringstream` and `std::basic_fstream`,
which know how to correctly swap the associated stream buffers.

### Parameters

- **other** — another stream to exchange the state with

### Return value

`*this`

---
*Source: https://en.cppreference.com/w/cpp/io/basic_iostream/swap*
