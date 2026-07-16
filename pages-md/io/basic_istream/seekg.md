# std::basic_istream<CharT,Traits>::seekg

```cpp
basic_istream& seekg( pos_type pos );  // (1)
basic_istream& seekg( off_type off, std::ios_base::seekdir dir );  // (2)
```

Sets input position indicator of the current associated `streambuf` object.

Before doing anything else, `seekg` clears `eofbit`.
*(since C++11)*

`seekg` behaves as UnformattedInputFunction, except that `gcount()` is not
affected. After constructing and checking the sentry object,

1) if `fail() != true`, sets the input position indicator to absolute (relative
   to the beginning of the file) value `pos`. Specifically, executes
   `rdbuf()->pubseekpos(pos, std::ios_base::in)` (`pubseekpos`, in turn, calls
   seekpos of the specific buffer, such as `basic_filebuf::seekpos`,
   `basic_stringbuf::seekpos`, or `strstreambuf::seekpos`). In case of failure,
   calls `setstate(std::ios_base::failbit)`.

2) if `fail() != true`, sets the input position indicator to position `off`,
   relative to position, defined by `dir`. Specifically, executes
   `rdbuf()->pubseekoff(off, dir, std::ios_base::in)`. In case of failure, calls
   `setstate(std::ios_base::failbit)`.

### Parameters

- **pos** — absolute position to set the input position indicator to
- **off** — relative position (positive or negative) to set the input position
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

`failure` if an error occurred (the error state flag is not `goodbit`) and
`exceptions()` is set to throw for that state.

If an internal operation throws an exception, it is caught and `badbit` is set.
If `exceptions()` is set for `badbit`, the exception is rethrown.

### Notes

`seekg(n)` is not necessarily equivalent to `seekg(n, ios::beg)`.
`std::basic_ifstream`, for example, requires the absolute position `n` to come
from `tellg()`.

### Example

```cpp
#include <iostream>
#include <sstream>
#include <string>

int main()
{
    std::string str = "Hello, world";
    std::istringstream in(str);
    std::string word1, word2;

    in >> word1;
    in.seekg(0); // rewind
    in >> word2;

    std::cout << "word1 = " << word1 << '\n'
              << "word2 = " << word2 << '\n';
}
```

Output:

```text
word1 = Hello,
word2 = Hello,
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 129 | C++98 | there was no way to indicate a failure | sets `failbit` on
      failure
  LWG 136 | C++98 | `seekg` could set the output stream | only sets the input
      stream
  LWG 537 | C++98 | the type of `off` was `off_type&` | corrected to `off_type`

### See also

- **tellg** — returns the input position indicator (public member function)
- **tellp** — returns the output position indicator (public member function of
  `std::basic_ostream<CharT,Traits>`)
- **seekp** — sets the output position indicator (public member function of
  `std::basic_ostream<CharT,Traits>`)
- **pubseekpos** — invokes `seekpos()` (public member function of
  `std::basic_streambuf<CharT,Traits>`)
- **seekpos [virtual]** — repositions the file position, using absolute
  addressing (virtual protected member function of
  `std::basic_filebuf<CharT,Traits>`)
- **seekpos [virtual]** — repositions the next pointer in the input sequence,
  output sequence, or both using absolute addressing (virtual protected member
  function of `std::basic_stringbuf<CharT,Traits,Allocator>`)
- **seekpos [virtual]** — repositions the next pointer in the input sequence,
  output sequence, or both using absolute addressing (virtual protected member
  function of `std::strstreambuf`)
- **pubseekoff** — invokes `seekoff()` (public member function of
  `std::basic_streambuf<CharT,Traits>`)
- **seekoff [virtual]** — repositions the file position, using relative
  addressing (virtual protected member function of
  `std::basic_filebuf<CharT,Traits>`)
- **seekoff [virtual]** — repositions the next pointer in the input sequence,
  output sequence, or both, using relative addressing (virtual protected member
  function of `std::basic_stringbuf<CharT,Traits,Allocator>`)
- **seekoff [virtual]** — repositions the next pointer in the input sequence,
  output sequence, or both, using relative addressing (virtual protected member
  function of `std::strstreambuf`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istream/seekg*
