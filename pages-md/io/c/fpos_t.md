# std::fpos_t

```cpp
typedef /* implementation-defined */ fpos_t;
```

`std::fpos_t` is a non-array complete object type, can be used to store (by
`std::fgetpos`) and restore (by `std::fsetpos`) the position and multibyte
parser state (if any) for a C stream.

The multibyte parser state of a wide-oriented C stream is represented by a
`std::mbstate_t` object, whose value is stored as part of the value of a
`std::fpos_t` object by `std::fgetpos`.

### See also

- **fgetpos** — gets the file position indicator (function)
- **fsetpos** — moves the file position indicator to a specific location in a
  file (function)
- **mbstate_t** — conversion state information necessary to iterate multibyte
  character strings (class)
- **fpos** — represents absolute position in a stream or a file (class template)

**C documentation for `fpos_t`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/fpos_t*
