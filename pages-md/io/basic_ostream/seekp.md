# std::basic_ostream<CharT,Traits>::seekp

```cpp
basic_ostream& seekp( pos_type pos );  // (1)
basic_ostream& seekp( off_type off, std::ios_base::seekdir dir );  // (2)
```

Sets the output position indicator of the current associated `streambuf` object.

Behaves as UnformattedOutputFunction (except without actually performing
output). After constructing and checking the sentry object,
*(since C++11)*

1) if `fail() != true`, sets the output position indicator to absolute (relative
   to the beginning of the file) value `pos` by calling
   `rdbuf()->pubseekpos(pos, std::ios_base::out)`. In case of failure, calls
   `setstate(std::ios_base::failbit)`.

2) if `fail() != true`, sets the output position indicator to offset `off`
   relative to `dir` by calling `rdbuf()->pubseekoff(off, dir,
   std::ios_base::out)`. In case of failure, calls
   `setstate(std::ios_base::failbit)`.

### Parameters

- **pos** — absolute position to set the output position indicator to
- **off** — relative position (positive or negative) to set the output position
  indicator to
- **dir** — defines base position to apply the relative offset to. It can be one
  of the following constants: Constant Explanation `beg` the beginning of a
  stream `end` the ending of a stream `cur` the current position of stream
  position indicator
- **`beg`** — the beginning of a stream
- **`end`** — the ending of a stream
- **`cur`** — the current position of stream position indicator

### Return value

`*this`

### Exceptions

1,2) May throw `std::ios_base::failure` in case of failure, if `exceptions() &
   failbit != 0`.

### Example

```cpp
#include <iostream>
#include <sstream>

int main()
{
    std::ostringstream os("hello, world");
    os.seekp(7);
    os << 'W';
    os.seekp(0, std::ios_base::end);
    os << '!';
    os.seekp(0);
    os << 'H';
    std::cout << os.str() << '\n';
}
```

Output:

```text
Hello, World!
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 129 | C++98 | there was no way to indicate a failure | sets `failbit` on
      failure
  LWG 136 | C++98 | `seekp` could set the input stream | only sets the output
      stream
  LWG 537 | C++98 | 1. the type of `pos` was `pos_type&` 2. the type of `off`
      was `off_type&` | 1. corrected to `pos_type` 2. corrected to `off_type`
  LWG 2341 | C++98 | the resolution of LWG issue 129 for overload (2) was
      removed | restored

### See also

- **tellp** — returns the output position indicator (public member function)
- **tellg** — returns the input position indicator (public member function of
  `std::basic_istream<CharT,Traits>`)
- **seekg** — sets the input position indicator (public member function of
  `std::basic_istream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ostream/seekp*
