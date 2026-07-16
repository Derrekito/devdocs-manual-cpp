# std::streamsize

```cpp
typedef /*implementation-defined*/ streamsize;
```

The type `std::streamsize` is an implementation-defined signed integral type
used to represent the number of characters transferred in an I/O operation or
the size of an I/O buffer. It is used as a signed counterpart of `std::size_t`,
similar to the POSIX type `ssize_t`.

Except in the constructors of `std::strstreambuf`, negative values of
`std::streamsize` are never used.

### See also

- **gcount** — returns number of characters extracted by last unformatted input
  operation (public member function of `std::basic_istream<CharT,Traits>`)
- **ignore** — extracts and discards characters until the given character is
  found (public member function of `std::basic_istream<CharT,Traits>`)
- **read** — extracts blocks of characters (public member function of
  `std::basic_istream<CharT,Traits>`)
- **write** — inserts blocks of characters (public member function of
  `std::basic_ostream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/streamsize*
