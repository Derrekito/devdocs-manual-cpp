# std::basic_stringstream

`std::stringstream` — build or parse strings with stream operators
instead of manual concatenation or parsing. `basic_stringstream`
wraps a `std::basic_string` and exposes it through the full
`basic_iostream` interface, so `operator<<`/`operator>>` and every
iostream manipulator (`std::hex`, `std::setw`, ...) work on it exactly
as they do on `std::cin`/`std::cout`.

```cpp skip
std::stringstream ss;               // empty, both directions
std::stringstream ss{"42 abc"};     // pre-loaded with initial contents
ss << x;                            // format x into the stream
ss >> y;                            // parse y out of the stream
ss.str()                            // a copy of the whole buffered string
ss.str(s)                           // replace the buffered string with s
ss.view()                           // (C++20) a non-owning view, no copy
```

`std::wstringstream` is the `wchar_t` typedef.

### What you provide

- **CharT** — the character type (`char` for `std::stringstream`,
  `wchar_t` for `std::wstringstream`).
- **Traits** — character traits, defaults to `std::char_traits<CharT>`.
- **Allocator** — allocator for the internal string, defaults to
  `std::allocator<CharT>`.

### Member functions

Own members (beyond what's inherited):

| Member | What it does |
| --- | --- |
| `str()` | returns a copy of the buffered string |
| `str(s)` | replaces the buffered string with `s` |
| `view()` (C++20) | non-owning view over the buffered string, no copy |
| `rdbuf()` | the underlying `basic_stringbuf` |
| `swap(other)` (C++11) | swaps two string streams |

Everything else is inherited from `basic_iostream` (and, through it,
`basic_istream`/`basic_ostream`/`basic_ios`/`ios_base`): formatted
`operator<<`/`operator>>`; unformatted `get`, `peek`, `unget`,
`putback`, `getline`, `ignore`, `read`, `write`, `put`, `gcount`;
positioning `tellg`/`seekg`/`tellp`/`seekp`; state `good`/`eof`/`fail`/
`bad`/`operator bool`/`clear`; formatting `flags`/`setf`/`precision`/
`width`; and locale handling via `imbue`/`getloc`.

### Guarantees and costs

- Constructing from a string, or calling `str(s)`, copies `s` into the
  stream's internal buffer.
- `str()` returns a **copy** of the current buffer on every call —
  building a large result by calling `str()` repeatedly (instead of
  accumulating with `operator<<` and calling `str()` once at the end)
  pays for that copy each time.
- `view()` (C++20) reads the buffer without copying, when a
  non-owning view is all you need.

### Gotchas

- Prefer accumulating output with `operator<<` and reading the result
  with a single `str()` (or `view()`) call — calling `str()` inside a
  loop repeatedly copies the whole buffer.
- Extracting with `operator>>` leaves the underlying buffer untouched:
  `str()` still returns the *entire* original contents even after
  you've read part of it out with `>>`.

### Example

```cpp
#include <iostream>
#include <sstream>
#include <string>

int main()
{
    std::stringstream ss;
    ss << "Value:" << ' ' << 42 << ' ' << 3.5;

    std::string label;
    int n;
    double d;
    ss >> label >> n >> d;

    std::cout << label << ' ' << n << ' ' << d << '\n';
    std::cout << "buffered: " << ss.str() << '\n';
}
```

```text
Value: 42 3.5
buffered: Value: 42 3.5
```

### Reference

```cpp skip
template<
    class CharT,
    class Traits = std::char_traits<CharT>,
    class Allocator = std::allocator<CharT>
> class basic_stringstream
    : public basic_iostream<CharT, Traits>;
```

Typedefs: `std::stringstream` is `basic_stringstream<char>`;
`std::wstringstream` is `basic_stringstream<wchar_t>`. Member types
`char_type`, `traits_type`, `int_type`, `pos_type`, `off_type`,
`allocator_type` name `CharT`, `Traits`, `Traits::int_type`,
`Traits::pos_type`, `Traits::off_type`, and `Allocator` respectively;
the program is ill-formed if `Traits::char_type` is not `CharT`.
Internally the class wraps a `basic_stringbuf<CharT, Traits,
Allocator>`, whose full unique interface (`str`, `view`, `swap`) is
exposed directly on the stream.

### See also

- **basic_istringstream** — input-only string stream
- **basic_ostringstream** — output-only string stream
- **basic_stringbuf** — the underlying string buffer this class wraps
- **basic_ifstream** — input stream over a file, same iostream interface
- **basic_ofstream** — output stream over a file, same iostream interface

---
*Source: https://en.cppreference.com/w/cpp/io/basic_stringstream*
