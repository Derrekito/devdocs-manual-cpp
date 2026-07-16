# std::basic_ifstream<CharT,Traits>::open

```cpp
void open( const char* filename,
           std::ios_base::openmode mode
               = std::ios_base::in );  // (1)
void open( const std::filesystem::path::value_type* filename,
           std::ios_base::openmode mode
               = std::ios_base::in );  // (2) (since C++17)
void open( const std::string& filename,
           std::ios_base::openmode mode
               = std::ios_base::in );  // (3) (since C++11)
void open( const std::filesystem::path& filename,
           std::ios_base::openmode mode
               = std::ios_base::in );  // (4) (since C++17)
```

Opens and associates the file with name `filename` with the file stream.

Calls `clear()` on success. Calls `setstate(failbit)` on failure.

1,2) Effectively calls `rdbuf()->open(filename, mode | ios_base::in)` (see
   `std::basic_filebuf::open` for the details on the effects of that call).
   Overload (2) is only provided if `std::filesystem::path::value_type` is not
   `char`.(since C++17)

3,4) Effectively calls (1,2) as if by `open(filename.c_str(), mode)`.

### Parameters

- **filename** — the name of the file to be opened
- **mode** — specifies stream open mode. It is a BitmaskType, the following
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

### Return value

(none)

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 22 | C++98 | it was unclear how error state changes upon a successful open
      | the error state is unchanged
  LWG 409 | C++98 | the error state was unchanged upon a successful open | it is
      cleared[1]

1. The resolution of LWG issue #22 is overriden.

### See also

- **is_open** — checks if the stream has an associated file (public member
  function)
- **close** — closes the associated file (public member function)
- **open** — opens a file and configures it as the associated character sequence
  (public member function of `std::basic_filebuf<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ifstream/open*
