# std::basic_spanbuf<CharT,Traits>::basic_spanbuf

```cpp
basic_spanbuf() : basic_spanbuf(std::ios_base::in | std::ios_base::out) {}  // (1) (since C++23)
explicit basic_spanbuf( std::ios_base::openmode which )
    : basic_spanbuf(std::span<CharT>{}, which) {}  // (2) (since C++23)
explicit basic_spanbuf( std::span<CharT> buf, std::ios_base::openmode which =
                            std::ios_base::in | std::ios_base::out );  // (3) (since C++23)
basic_spanbuf( const basic_spanbuf& ) = delete;  // (4) (since C++23)
basic_spanbuf( basic_spanbuf&& rhs );  // (5) (since C++23)
```

1) Default constructor. Creates a `basic_spanbuf` that has no underlying buffer
   and is opened for both input and output. The pointers to get and put area are
   set to the null pointer value.

2) Same as (1), except that the `basic_spanbuf` is opened in mode specified by
   `which`.

3) Creates a `basic_spanbuf` that manages the underlying buffer referenced by
   `buf` (or has no underlying buffer if `buf` is empty) and is opened in mode
   specified by `which`. The pointers to get and put area are set as following,
   or to the null pointer value if not mentioned in the table: Set bits in open
   mode (affecting pointers to get area) Return value after setting `eback()`
   `gptr()` `egptr()` `std::ios_base::in` `s.data()` `s.data()` `s.data() +
   s.size()` Set bits in open mode (affecting pointers to put area) Return value
   after setting `pbase()` `pptr()` `epptr()` `std::ios_base::out &&
   !std::ios_base::ate` `s.data()` `s.data()` `s.data() + s.size()`
   `std::ios_base::out && std::ios_base::ate` `s.data()` `s.data() + s.size()`
   `s.data() + s.size()`

4) Copy constructor is deleted. `basic_spanbuf` is not copyable.

5) Move constructor. Move-constructs the `std::basic_streambuf` base subobject
   from that of `rhs`. The pointers to get and put area, the open mode, and the
   underlying buffer (if any) are identical to those in `rhs` before
   construction. It is implementation-defined whether `rhs` still holds the
   underlying buffer after the move-construction.

### Parameters

- **buf** — a `std::span` referencing the underlying buffer
- **rhs** — another `basic_spanbuf`
- **which** — specifies stream open mode. It is bitmask type, the following
  constants are defined: Constant Explanation `app` seek to the end of stream
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

### Notes

These constructors are typically called by the constructors of
`std::basic_ispanstream`, `std::basic_ospanstream`, and `std::basic_spanstream`.

### Example

### See also

- **(constructor) (C++23)** — constructs the `basic_ispanstream` (public member
  function of `std::basic_ispanstream<CharT,Traits>`)
- **(constructor) (C++23)** — constructs the `basic_ospanstream` (public member
  function of `std::basic_ospanstream<CharT,Traits>`)
- **(constructor) (C++23)** — constructs the `basic_spanstream` (public member
  function of `std::basic_spanstream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_spanbuf/basic_spanbuf*
