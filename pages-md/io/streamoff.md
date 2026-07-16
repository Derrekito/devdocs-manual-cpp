# std::streamoff

```cpp
typedef /*implementation-defined*/ streamoff;
```

The type `std::streamoff` is an implementation-defined signed integral(since
C++11) type of sufficient size to represent the maximum possible file size
supported by the operating system. Typically, this is an alias for `long
long`.(since C++11)

It is used to represent offsets from stream positions (values of type
`std::fpos`). A `std::streamoff` value constructed from `-1` is also used to
represent error conditions by some of the I/O library functions.

### Relationship with std::fpos

- the difference between two `std::fpos` objects is a value of type
  `std::streamoff`
- a value of type `std::streamoff` may be added or subtracted from `std::fpos`
  yielding a different `std::fpos`.
- a value of type `std::fpos` is implicitly convertible to `std::streamoff` (the
  conversion result is the offset from the beginning of the file).
- a value of type `std::fpos` is constructible from a value of type
  `std::streamoff`

### See also

- **fpos** — represents absolute position in a stream or a file (class template)
- **seekg** — sets the input position indicator (public member function of
  `std::basic_istream<CharT,Traits>`)
- **seekp** — sets the output position indicator (public member function of
  `std::basic_ostream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/streamoff*
