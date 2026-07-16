# std::basic_fstream

`std::fstream` — a `basic_iostream` over a file: open it in the constructor,
check with `is_open()` or the stream's own bool, and it closes itself on
destruction (RAII), so you rarely call `close()` by hand. It interfaces a
`std::basic_filebuf` with the `basic_iostream` interface, giving one object
both `<<` and `>>` over the same file.

```cpp skip
std::fstream f;                       // not yet associated
std::fstream f{path};                 // default mode: ios::in | ios::out
std::fstream f{path, std::ios::in | std::ios::out | std::ios::trunc};
f.is_open()                           // true iff a file is currently open
f.open(path, mode);                   // (re)open, after default construction
if (f) ...                            // true iff no error bit is set
```

`std::wfstream` is the `wchar_t` typedef.

### What you provide

- **CharT** — the character type (`char` for `std::fstream`, `wchar_t` for
  `std::wfstream`).
- **Traits** — character traits, defaults to `std::char_traits<CharT>`.

### Member functions

Own members (beyond what's inherited):

| Member | What it does |
| --- | --- |
| (constructor) | default, or open a file immediately |
| (destructor) | closes the file if open — RAII |
| `operator=` (C++11) | move-assigns from another `basic_fstream` |
| `is_open()` | true if a file is currently associated |
| `open(filename, mode)` | opens `filename`; sets `failbit` on failure |
| `close()` | closes the associated file |
| `rdbuf()` | the underlying `basic_filebuf` |
| `native_handle()` (C++26) | the OS-level file handle |
| `swap(other)` (C++11) | swaps two file streams |

Everything else is inherited from `basic_iostream` (and, through it,
`basic_istream`/`basic_ostream`/`basic_ios`/`ios_base`): formatted
`operator<<`/`operator>>`; unformatted `get`, `peek`, `unget`, `putback`,
`getline`, `ignore`, `read`, `write`, `put`, `gcount`; positioning
`tellg`/`seekg`/`tellp`/`seekp`; state `good`/`eof`/`fail`/`bad`/
`operator bool`/`clear`; formatting `flags`/`setf`/`precision`/`width`; and
locale handling via `imbue`/`getloc`.

### Guarantees and costs

- The default open mode is `std::ios_base::in | std::ios_base::out` — no
  `trunc`, so it behaves like `fopen(..., "r+")`: the file must already
  exist and its contents survive; add `trunc` to create a fresh file, `app`
  to always write at the end.
- Read and write share one file position in typical implementations — after
  writing, a `seekg`/`seekp` (either moves the shared position) is usually
  needed before reading back what you just wrote.
- On open failure, `open()` (and the opening constructor) sets `failbit`
  rather than throwing; check `is_open()` or the stream's bool conversion.
- The destructor closes the file automatically; `close()` exists for when
  you need to release the file before the stream itself goes out of scope.

### Gotchas

- Forgetting `trunc` when you actually want a fresh file: the default
  `in | out` mode fails outright if the file doesn't exist yet.
- Forgetting to seek between a write and the following read (or vice
  versa) silently reads or overwrites the wrong bytes, because both
  directions share the same position.
- Constructing with a bad path or mode combination doesn't throw — check
  `is_open()`/the stream's bool before using it.

### Example

```cpp
#include <filesystem>
#include <fstream>
#include <iostream>
#include <string>

int main()
{
    namespace fs = std::filesystem;
    fs::path p = fs::temp_directory_path() / "basic_fstream_demo.bin";

    std::fstream s{p, std::ios::binary | std::ios::trunc
                       | std::ios::in | std::ios::out};
    if (!s.is_open())
    {
        std::cout << "failed to open\n";
    }
    else
    {
        double d = 3.14;
        s.write(reinterpret_cast<char*>(&d), sizeof d);
        s << 123 << "abc";

        s.seekg(0);   // rewind the shared position before reading back
        d = 0.0;
        s.read(reinterpret_cast<char*>(&d), sizeof d);
        int n;
        std::string word;
        if (s >> n >> word)
            std::cout << d << ' ' << n << ' ' << word << '\n';
    }

    fs::remove(p);
}
```

```text
3.14 123 abc
```

### Reference

```cpp skip
template<
    class CharT,
    class Traits = std::char_traits<CharT>
> class basic_fstream : public std::basic_iostream<CharT, Traits>;
```

Typedefs: `std::fstream` is `basic_fstream<char>`; `std::wfstream` is
`basic_fstream<wchar_t>`. Member types `char_type`, `traits_type`,
`int_type`, `pos_type`, `off_type` name `CharT`, `Traits`,
`Traits::int_type`, `Traits::pos_type`, `Traits::off_type`; `native_handle_type`
(C++26) is an implementation-defined, TriviallyCopyable, semiregular type. A
typical implementation holds one non-derived data member: a
`std::basic_filebuf<CharT, Traits>`. Non-member `std::swap` (C++11)
specializes for two `basic_fstream`s.

### See also

- **basic_ifstream** — read-only file stream
- **basic_ofstream** — write-only file stream
- **basic_iostream** — the interface this class implements over a file
- **getline** — read data from an I/O stream into a string

---
*Source: https://en.cppreference.com/w/cpp/io/basic_fstream*
