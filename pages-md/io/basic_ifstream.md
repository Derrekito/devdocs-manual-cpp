# std::basic_ifstream

`std::ifstream` — a `basic_istream` over a file: open it in the constructor,
check with `is_open()` or the stream's own bool, and it closes itself on
destruction (RAII), so you rarely call `close()` by hand. It interfaces a
`std::basic_filebuf` with the `basic_istream` interface, so every `>>`,
`get`, `getline`, ... works exactly as it does on `std::cin`.

```cpp skip
std::ifstream f;                    // not yet associated with a file
std::ifstream f{path};              // open path, default mode ios::in
std::ifstream f{path, mode};        // open with an explicit openmode
f.is_open()                         // true iff a file is currently open
f.open(path, mode);                 // (re)open, after a default construction
if (f) ...                          // true iff no error bit is set
```

`std::wifstream` is the `wchar_t` typedef.

### What you provide

- **CharT** — the character type (`char` for `std::ifstream`, `wchar_t` for
  `std::wifstream`).
- **Traits** — character traits, defaults to `std::char_traits<CharT>`.

### Member functions

Own members (beyond what's inherited):

| Member | What it does |
| --- | --- |
| (constructor) | default, or open a file immediately |
| (destructor) | closes the file if open — RAII |
| `operator=` (C++11) | move-assigns from another `basic_ifstream` |
| `is_open()` | true if a file is currently associated |
| `open(filename, mode)` | opens `filename`; sets `failbit` on failure |
| `close()` | closes the associated file |
| `rdbuf()` | the underlying `basic_filebuf` |
| `native_handle()` (C++26) | the OS-level file handle |
| `swap(other)` (C++11) | swaps two file streams |

Everything else is inherited from `basic_istream` (and, through it,
`basic_ios`/`ios_base`): formatted `operator>>`; unformatted `get`, `peek`,
`unget`, `putback`, `getline`, `ignore`, `read`, `readsome`, `gcount`;
positioning `tellg`/`seekg`; state `good`/`eof`/`fail`/`bad`/
`operator bool`/`clear`; formatting `flags`/`setf`/`precision`/`width`; and
locale handling via `imbue`/`getloc`.

### Guarantees and costs

- The default open mode is `std::ios_base::in`; passing another mode still
  implicitly ORs in `in`.
- On open failure — file missing, no permission, ... — `open()` (and the
  opening constructor) sets `failbit` rather than throwing; check
  `is_open()` or the stream's bool conversion before reading, don't assume
  the constructor succeeded.
- The destructor closes the file automatically; `close()` exists for when
  you need to release the file before the stream itself goes out of scope.

### Gotchas

- Constructing with a bad path doesn't throw — it silently leaves the
  stream in a fail state. `std::ifstream f{path}; if (!f) ...` catches it;
  skipping the check and reading anyway just yields more failed reads.
- `open()` on a stream that's already open fails (returns without
  effect and sets `failbit`) — `close()` it first if you're reusing the
  object for a different file.

### Example

```cpp
#include <filesystem>
#include <fstream>
#include <iostream>
#include <string>

int main()
{
    namespace fs = std::filesystem;
    fs::path p = fs::temp_directory_path() / "basic_ifstream_demo.txt";

    {
        std::ofstream out{p};
        out << "42 hello\n";
    }

    std::ifstream in{p};
    if (!in.is_open())
    {
        std::cout << "failed to open\n";
    }
    else
    {
        int n;
        std::string word;
        in >> n >> word;
        std::cout << n << ' ' << word << '\n';
    }

    fs::remove(p);
}
```

```text
42 hello
```

### Reference

```cpp skip
template<
    class CharT,
    class Traits = std::char_traits<CharT>
> class basic_ifstream : public std::basic_istream<CharT, Traits>;
```

Typedefs: `std::ifstream` is `basic_ifstream<char>`; `std::wifstream` is
`basic_ifstream<wchar_t>`. Member types `char_type`, `traits_type`,
`int_type`, `pos_type`, `off_type` name `CharT`, `Traits`,
`Traits::int_type`, `Traits::pos_type`, `Traits::off_type`; `native_handle_type`
(C++26) is an implementation-defined, TriviallyCopyable, semiregular type. A
typical implementation holds one non-derived data member: a
`std::basic_filebuf<CharT, Traits>`. Non-member `std::swap` (C++11)
specializes for two `basic_ifstream`s.

### See also

- **basic_ofstream** — the write side of the same interface
- **basic_fstream** — combined read/write over a file
- **basic_istream** — the interface this class implements over a file

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ifstream*
