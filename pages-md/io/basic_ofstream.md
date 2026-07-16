# std::basic_ofstream

`std::ofstream` — a `basic_ostream` over a file: open it in the constructor,
check with `is_open()` or the stream's own bool, and it closes itself on
destruction (RAII), so you rarely call `close()` by hand. It interfaces a
`std::basic_filebuf` with the `basic_ostream` interface, so every `<<`,
`put`, `write`, ... works exactly as it does on `std::cout`.

```cpp skip
std::ofstream f;                    // not yet associated with a file
std::ofstream f{path};              // open path, truncates existing content
std::ofstream f{path, std::ios::app};  // open, append instead of truncate
f.is_open()                         // true iff a file is currently open
f.open(path, mode);                 // (re)open, after a default construction
if (f) ...                          // true iff no error bit is set
```

`std::wofstream` is the `wchar_t` typedef.

### What you provide

- **CharT** — the character type (`char` for `std::ofstream`, `wchar_t` for
  `std::wofstream`).
- **Traits** — character traits, defaults to `std::char_traits<CharT>`.

### Member functions

Own members (beyond what's inherited):

| Member | What it does |
| --- | --- |
| (constructor) | default, or open a file immediately |
| (destructor) | closes the file if open — RAII |
| `operator=` (C++11) | move-assigns from another `basic_ofstream` |
| `is_open()` | true if a file is currently associated |
| `open(filename, mode)` | opens `filename`; sets `failbit` on failure |
| `close()` | closes the associated file |
| `rdbuf()` | the underlying `basic_filebuf` |
| `native_handle()` (C++26) | the OS-level file handle |
| `swap(other)` (C++11) | swaps two file streams |

Everything else is inherited from `basic_ostream` (and, through it,
`basic_ios`/`ios_base`): formatted `operator<<`; unformatted `put`, `write`;
positioning `tellp`/`seekp`; state `good`/`eof`/`fail`/`bad`/
`operator bool`/`clear`; formatting `flags`/`setf`/`precision`/`width`; and
locale handling via `imbue`/`getloc`.

### Guarantees and costs

- The default open mode is `std::ios_base::out` — and despite not naming
  `trunc`, opening this way has the *same effect* as `out | trunc`: an
  existing file's contents are discarded.
- Pass `std::ios::app` instead to seek to the end before every write and
  never discard existing contents — that's the `trunc` vs. `app` choice:
  overwrite from scratch, or always append.
- On open failure, `open()` (and the opening constructor) sets `failbit`
  rather than throwing; check `is_open()` or the stream's bool conversion
  before writing.
- The destructor closes the file automatically; `close()` exists for when
  you need to release the file before the stream itself goes out of scope.

### Gotchas

- Opening an existing file with the default mode silently erases it before
  you write anything — if you meant to keep old content, pass
  `std::ios::app` (or `std::ios::in | std::ios::out` on `basic_fstream`).
- Constructing with a bad path (e.g. a missing parent directory) doesn't
  throw — it leaves the stream in a fail state; check before writing.
- `open()` on a stream that's already open fails and sets `failbit` —
  `close()` first if reusing the object for a different file.

### Example

```cpp
#include <filesystem>
#include <fstream>
#include <iostream>
#include <string>

int main()
{
    namespace fs = std::filesystem;
    fs::path p = fs::temp_directory_path() / "basic_ofstream_demo.txt";

    {
        std::ofstream out{p};
        out << "first\n";
    }
    {
        std::ofstream out{p};              // default mode truncates
        out << "second\n";                 // "first" is gone
    }
    {
        std::ofstream out{p, std::ios::app};
        out << "third\n";                  // appended, "second" kept
    }

    std::ifstream in{p};
    std::string line;
    while (std::getline(in, line))
        std::cout << line << '\n';

    fs::remove(p);
}
```

```text
second
third
```

### Reference

```cpp skip
template<
    class CharT,
    class Traits = std::char_traits<CharT>
> class basic_ofstream : public std::basic_ostream<CharT, Traits>;
```

Typedefs: `std::ofstream` is `basic_ofstream<char>`; `std::wofstream` is
`basic_ofstream<wchar_t>`. Member types `char_type`, `traits_type`,
`int_type`, `pos_type`, `off_type` name `CharT`, `Traits`,
`Traits::int_type`, `Traits::pos_type`, `Traits::off_type`; `native_handle_type`
(C++26) is an implementation-defined, TriviallyCopyable, semiregular type. A
typical implementation holds one non-derived data member: a
`std::basic_filebuf<CharT, Traits>`. Non-member `std::swap` (C++11)
specializes for two `basic_ofstream`s.

### See also

- **basic_ifstream** — the read side of the same interface
- **basic_fstream** — combined read/write over a file
- **basic_ostream** — the interface this class implements over a file

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ofstream*
