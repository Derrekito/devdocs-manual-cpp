# std::basic_ostream

`std::ostream`, the type behind `std::cout` and every `<<` chain. It layers
formatted output (`operator<<`) and unformatted output (`put`, `write`) on
top of a stream buffer; every writable stream in the library (`ofstream`,
`ostringstream`, `cout`, ...) is a `basic_ostream` underneath. In typical
implementations it holds no data of its own beyond what it inherits.

```cpp skip
out << x;                            // formatted insert, any << overload
out << std::hex << std::setw(4);     // manipulators chain like values do
out.put(c);                          // one raw character, no formatting
out.write(data, n);                  // n raw characters, no formatting
out.flush();                         // push buffered bytes out now
if (out) ...                         // true iff no error bit is set
```

`std::wostream` is the `wchar_t` typedef.

### What you provide

- **CharT** — the character type (`char` for `std::ostream`, `wchar_t` for
  `std::wostream`).
- **Traits** — character traits, defaults to `std::char_traits<CharT>`.

### Member functions

Own members (beyond what's inherited):

| Member | What it does |
| --- | --- |
| `operator<<` | formatted insert: arithmetic, `bool`, pointers, manipulators |
| `put(c)` | inserts one raw character, unformatted |
| `write(s, n)` | inserts `n` raw characters from `s`, unformatted |
| `tellp()` | returns the output position |
| `seekp(pos)` | sets the output position |
| `flush()` | pushes buffered output to the underlying device |
| `swap(other)` (C++11) | swaps two streams, not the buffer (protected) |

Member class `sentry` prepares the stream before an output operation; you
don't call it directly. Non-member `print`/`println` (C++23) write a
`std::format`-style string straight to an ostream; `vprint_unicode`/
`vprint_nonunicode` (C++23) are the type-erased primitives those build on.

Everything else — state (`good`/`eof`/`fail`/`bad`/`operator bool`/`clear`),
format flags (`flags`/`setf`/`precision`/`width`), locale (`imbue`) — is
inherited from **basic_ios** and **ios_base**.

### Guarantees and costs

- `operator<<` is selected by overload resolution at compile time and
  formats according to the flags held in the base `ios_base` (width, fill,
  base, precision, ...); a formatted insert resets `width` to 0 afterward, so
  `setw` applies to only the next field.
- `put`/`write` ignore those flags entirely — no padding, no base
  conversion — which is why they're "unformatted".
- Once an error bit (`failbit`/`badbit`) is set, every later `<<`, `put`, or
  `write` is a no-op until `clear()` runs; the state does not reset itself.
- `flush()` (and `std::endl`, which is `'\n'` plus a flush) forces buffered
  bytes out immediately — fine occasionally, costly in a tight loop.

### Gotchas

- `operator<<(bool)` prints `0`/`1` unless `std::boolalpha` is set — a
  frequent surprise when logging a flag.
- A chain never stops on error: `out << a << b << c;` keeps "running" even
  after `a` fails, because each `<<` still returns the stream — check `out`
  (or `out.good()`) after the chain, not mid-chain.
- Prefer `'\n'` over `std::endl` inside loops; the automatic flush defeats
  buffering.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <sstream>

void report(std::ostream& out, int code)
{
    out << "code=" << std::setw(4) << std::setfill('0') << code;
    out.put(' ');
    out << std::hex << "0x" << code << '\n';
}

int main()
{
    std::ostringstream oss;
    report(oss, 255);
    std::cout << oss.str();
}
```

```text
code=0255 0xff
```

### Reference

```cpp skip
template<
    class CharT,
    class Traits = std::char_traits<CharT>
> class basic_ostream : virtual public std::basic_ios<CharT, Traits>;
```

Typedefs: `std::ostream` is `basic_ostream<char>`; `std::wostream` is
`basic_ostream<wchar_t>`. Member types `char_type`, `traits_type`,
`int_type`, `pos_type`, `off_type` name `CharT`, `Traits`,
`Traits::int_type`, `Traits::pos_type`, `Traits::off_type`; the program is
ill-formed if `Traits::char_type` is not `CharT`.

Six global `basic_ostream` objects exist: `cout`/`wcout` (write to `stdout`),
`cerr`/`wcerr` (write to `stderr`, unbuffered), `clog`/`wclog` (write to
`stderr`, buffered).

### See also

- **basic_istream** — the read side of the same interface
- **basic_stringstream** — the same interface over an in-memory string
- **basic_ofstream** — the same interface over a file
- **cout**
- **cerr**

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ostream*
