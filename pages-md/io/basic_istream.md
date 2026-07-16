# std::basic_istream

`std::istream`, behind `std::cin` and `>>`. Reads fail **silently**: a bad
extraction doesn't throw or return an error code, it just sets a fail bit on
the stream (and writes zero into the target) and leaves the rest of the
input untouched — always wrap the read in a check, `if (in >> x)`, rather
than trusting the value blindly. It layers formatted input (`operator>>`)
and unformatted input (`get`, `read`, ...) on top of a stream buffer; every
readable stream in the library (`ifstream`, `istringstream`, `cin`, ...) is a
`basic_istream` underneath.

```cpp skip
if (in >> x) ...                // idiomatic: true iff the extraction worked
in >> x >> y;                   // chained, skips leading whitespace
in.get(c);                      // one raw character, whitespace included
std::getline(in, line);         // a whole line into a std::string
in.ignore(n, delim);             // discard up to n chars or until delim
in.read(buf, n);                // n raw bytes, no formatting
in.gcount();                    // count from the last unformatted read
```

`std::wistream` is the `wchar_t` typedef.

### What you provide

- **CharT** — the character type (`char` for `std::istream`, `wchar_t` for
  `std::wistream`).
- **Traits** — character traits, defaults to `std::char_traits<CharT>`.

### Member functions

Own members (beyond what's inherited):

| Member | What it does |
| --- | --- |
| `operator>>` | formatted extract: arithmetic, `bool`, pointers, manipulators |
| `get(c)` / `get(buf, n)` | one or more raw characters, whitespace included |
| `peek()` | looks at the next character without extracting it |
| `unget()` / `putback(c)` | pushes the last (or given) character back on |
| `getline(buf, n, delim)` | extracts into a buffer until `delim` or `n` chars |
| `ignore(n, delim)` | discards up to `n` characters or until `delim` |
| `read(buf, n)` | `n` raw bytes, unformatted |
| `readsome(buf, n)` | like `read`, but only what's already buffered |
| `gcount()` | characters extracted by the last **unformatted** read |
| `tellg()` / `seekg(pos)` | returns / sets the input position |
| `sync()` | synchronizes with the underlying device |
| `swap(other)` (C++11) | swaps two streams, not the buffer (protected) |

Member class `sentry` prepares the stream before an input operation; you
don't call it directly. The free function `std::getline(istream&, string&)`
is the usual way to read a whole line into a `std::string`.

Everything else — state (`good`/`eof`/`fail`/`bad`/`operator bool`/`clear`),
format flags (`flags`/`setf`/`precision`/`width`), locale (`imbue`) — is
inherited from **basic_ios** and **ios_base**.

### Guarantees and costs

- `operator>>` skips leading whitespace by default (governed by the
  `skipws` flag); unformatted reads (`get`, `read`, ...) do not.
- On a failed extraction, `operator>>` sets `failbit` (and `eofbit` if input
  ran out), writes zero into the target, and never throws unless you've
  opted in via `exceptions()`.
- `gcount()` only reflects **unformatted** reads (`get`, `getline`, `ignore`,
  `read`, `readsome`, ...); `operator>>` doesn't update it.
- Once an error bit is set, every later formatted or unformatted read is a
  no-op until `clear()` runs — the state does not reset itself.

### Gotchas

- The classic bug: `while (in >> x) { ... }` is correct; `while (!in.eof())`
  is not. `eof()` only reports the state set by the *last* operation — if
  a `get()` consumes the final byte of a file, `eof()` is still `false`;
  only the *next* read, which finds nothing, sets `eofbit`. Looping on
  `!eof()` runs the body one extra time on exhausted input.
- `if (in >> x)` tests the stream's state, not `x` — it correctly rejects a
  read that landed on a stream already in a fail state, even if `x` looks
  plausible.
- Once an error bit is set, it stays set until an explicit `clear()` —
  nothing about a later successful-looking operation resets it for you.

### Example

```cpp
#include <iostream>
#include <sstream>
#include <string>

int main()
{
    std::istringstream in{"42 abc oops"};

    int n;
    std::string word;
    if (in >> n >> word)
        std::cout << "read: " << n << ' ' << word << '\n';

    double d;
    if (!(in >> d))
        std::cout << "extraction failed, stream flagged bad\n";
}
```

```text
read: 42 abc
extraction failed, stream flagged bad
```

### Reference

```cpp skip
template<
    class CharT,
    class Traits = std::char_traits<CharT>
> class basic_istream : virtual public std::basic_ios<CharT, Traits>;
```

Typedefs: `std::istream` is `basic_istream<char>`; `std::wistream` is
`basic_istream<wchar_t>`. Member types `char_type`, `traits_type`,
`int_type`, `pos_type`, `off_type` name `CharT`, `Traits`,
`Traits::int_type`, `Traits::pos_type`, `Traits::off_type`; the program is
ill-formed if `Traits::char_type` is not `CharT`. The only non-inherited
data member, in most implementations, is the value returned by `gcount()`.

Two global `basic_istream` objects exist: `cin`/`wcin`, reading from
`stdin`.

### See also

- **basic_ostream** — the write side of the same interface
- **basic_stringstream** — the same interface over an in-memory string
- **basic_ifstream** — the same interface over a file
- **cin**

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istream*
