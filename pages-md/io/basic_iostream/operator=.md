# std::basic_iostream<CharT,Traits>::operator=

```cpp
basic_iostream& operator=( const basic_iostream& other ) = delete;  // (1)
protected:
basic_iostream& operator=( basic_iostream&& other );  // (2) (since C++11)
```

Assigns another stream object.

1) Copy assignment is not allowed.

2) Move assigns another stream object. Effectively calls `swap(rhs)`. This move
   assignment operator is protected: it is called by the move assignment
   operators of the derived stream classes `std::basic_stringstream` and
   `std::basic_fstream` which know how to properly move-assign the associated
   stream buffers.

### Parameters

- **other** — another stream to assign state of

### Return value

`*this`

### See also

- **(constructor)** — constructs the object (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_iostream/operator%3D*
