# std::basic_ofstream<CharT,Traits>::basic_ofstream

```cpp
basic_ofstream();  // (1)
explicit basic_ofstream( const char* filename,
                         std::ios_base::openmode mode
                             = std::ios_base::out );  // (2)
explicit basic_ofstream( const std::filesystem::path::value_type* filename,
                         std::ios_base::openmode mode
                             = std::ios_base::out );  // (3) (since C++17)
explicit basic_ofstream( const std::string& filename,
                         std::ios_base::openmode mode
                             = std::ios_base::out );  // (4) (since C++11)
template< class FsPath >
explicit basic_ofstream( const FsPath& filename,
                         std::ios_base::openmode mode
                             = std::ios_base::out );  // (5) (since C++17)
basic_ofstream( basic_ofstream&& other );  // (6) (since C++11)
basic_ofstream( const basic_ofstream& rhs ) = delete;  // (7) (since C++11)
```

Constructs new file stream.

1) Default constructor: constructs a stream that is not associated with a file:
   default-constructs the `std::basic_filebuf` and constructs the base with the
   pointer to this default-constructed `std::basic_filebuf` member.

2,3) First, performs the same steps as the default constructor, then associates
   the stream with a file by calling `rdbuf()->open(filename, mode |
   std::ios_base::out)` (see `std::basic_filebuf::open` for the details on the
   effects of that call). If the `open()` call returns a null pointer, sets
   `setstate(failbit)`. Overload (3) is only provided if
   `std::filesystem::path::value_type` is not `char`.(since C++17)

4,5) Same as `basic_ofstream(filename.c_str(), mode)`. (5) participates in
   overload resolution only if `FsPath` is `std::filesystem::path`.(since C++17)
   Note that despite the default mode being `out`, the effects are identical to
   the effects of `out | trunc` as described in `std::filebuf::open`

6) Move constructor. First, move-constructs the base class from `other` (which
   does not affect the `rdbuf()` pointer), then move-constructs the
   `std::basic_filebuf` member, then calls `this->set_rdbuf()` to install the
   new `basic_filebuf` as the `rdbuf()` pointer in the base class.

7) The copy-constructor is deleted: this class is not copyable.

### Parameters

- **filename** — the name of the file to be opened
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
- **other** — another file stream to use as source

### Example

```cpp
#include <fstream>
#include <string>
#include <utility>

int main()
{
    std::ofstream f0;
    std::ofstream f1("test.bin", std::ios::binary);
    std::string name = "example.txt";
    std::ofstream f2(name);
    std::ofstream f3(std::move(f1));
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3430 | C++17 | `std::filesystem::path` overload led to unwanted
      conversions | avoided by making it a template

### See also

- **open** — opens a file and associates it with the stream (public member
  function)
- **open** — opens a file and configures it as the associated character sequence
  (public member function of `std::basic_filebuf<CharT,Traits>`)
- **set_rdbuf** — replaces the `rdbuf` without clearing its error state
  (protected member function)
- **(constructor)** — constructs the object (public member function of
  `std::basic_ostream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ofstream/basic_ofstream*
