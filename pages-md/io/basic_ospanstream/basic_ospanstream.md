# std::basic_ospanstream<CharT,Traits>::basic_ospanstream

```cpp
explicit basic_ospanstream( std::span<CharT> s, std::ios_base::openmode mode =
                                std::ios_base::out );  // (1) (since C++23)
basic_ospanstream( basic_ospanstream&& rhs );  // (2) (since C++23)
basic_ospanstream( const basic_ospanstream& ) = delete;  // (3) (since C++23)
```

Constructs a new `basic_ospanstream`.

1) Uses the storage referenced by `s` as initial underlying buffer of the
   wrapped `std::basic_spanbuf` device. The wrapped `std::basic_spanbuf` object
   is constructed as `basic_spanbuf<Char, Traits>(s, mode |
   std::ios_base::out)`.

2) Move constructor. Move constructs the `std::basic_ostream` base subobject and
   the wrapped `std::basic_spanbuf` from those of `rhs`, and then calls
   `set_rdbuf` with the address of the wrapped `std::basic_spanbuf` in `*this`
   to install it.

3) Copy constructor is deleted. `basic_ospanstream` is not copyable.

### Parameters

- **s** — `std::span` referencing the storage to be use as initial underlying
  buffer of stream
- **mode** — specifies stream open mode. Following constants and bit-wise OR
  between them may be used: Constant Explanation `app` seek to the end of stream
  before each write `binary` open in binary mode `in` open for reading `out`
  open for writing `trunc` discard the contents of the stream when opening `ate`
  seek to the end of stream immediately after open `noreplace` (C++23) open in
  exclusive mode
- **`app`** — seek to the end of stream before each write
- **`binary`** — open in binary mode
- **`in`** — open for reading
- **`out`** — open for writing
- **`trunc`** — discard the contents of the stream when opening
- **`ate`** — seek to the end of stream immediately after open
- **`noreplace` (C++23)** — open in exclusive mode
- **other** — another `basic_ospanstream` to be moved from

### Exceptions

May throw implementation-defined exceptions.

### Example

### See also

- **(constructor) (C++23)** — constructs a `basic_spanbuf` object (public member
  function of `std::basic_spanbuf<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ospanstream/basic_ospanstream*
