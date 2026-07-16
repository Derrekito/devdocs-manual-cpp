# std::basic_iostream<CharT,Traits>::basic_iostream

```cpp
explicit basic_iostream( std::basic_streambuf<CharT,Traits>* sb );  // (1)
basic_iostream( const basic_iostream& other ) = delete;  // (2) (since C++11)
protected:
basic_iostream( basic_iostream&& other );  // (3) (since C++11)
```

Constructs new stream object.

1) Initializes with streambuf `sb`. The base classes are initialized as
   `basic_istream<CharT,Traits>(sb)` and `basic_ostream<CharT,Traits>(sb)`.
   After the call `rdbuf() == sb` and `gcount() == 0`.

2) Copy construction is not allowed.

3) Move constructor: move-constructs the first base class `basic_istream` as
   `basic_istream<CharT,Traits>(std::move(rhs));`, which in turn move-constructs
   and initializes the virtual base `std::basic_ios`. The initialization of the
   other base, `basic_ostream`, is implementation-defined (e.g., a protected
   default constructor may be added to `std::basic_ostream`, which does nothing)
   because move-construction cannot use `rhs` twice. This move constructor is
   protected: it is called by the move constructors of the derived stream
   classes `std::basic_fstream` and `std::basic_stringstream` before they
   move-construct and associate the stream buffer.

### Parameters

- **sb** — streambuf to initialize with
- **other** — another stream to initialize with

### See also

- **operator= (C++11)** — move-assigns another `basic_iostream` (protected
  member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_iostream/basic_iostream*
