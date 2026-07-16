# Standard library header <fstream>

`<fstream>` provides file-based I/O streams — the on-disk counterpart
to `<sstream>`'s in-memory streams. Construct one with a filename (or
call `open`), then read or write through it with the same
`<<`/`>>`/`getline` interface as `std::cin`/`std::cout`. Each of the
three high-level stream classes owns a `basic_filebuf`, which does
the actual buffered reading/writing against the OS file.

### Stream classes

- **`basic_ifstream`** (**`ifstream`**, **`wifstream`**) — file input
- **`basic_ofstream`** (**`ofstream`**, **`wofstream`**) — file output
- **`basic_fstream`** (**`fstream`**, **`wfstream`**) — file
  input and output

### Buffer

- **`basic_filebuf`** (**`filebuf`**, **`wfilebuf`**) — the raw
  device backing the stream classes above; `rdbuf()` on any of them
  returns a pointer to one

### Functions

- **`std::swap`** (C++11) — overloads for each class above, swapping
  two streams' (or buffers') contents

---
*Source: https://en.cppreference.com/w/cpp/header/fstream*
