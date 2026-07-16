# std::fgetwc

```cpp
std::wint_t fgetwc( std::FILE* stream );
std::wint_t getwc( std::FILE* stream );
```

Reads the next wide character from the given input stream. `getwc()` may be
implemented as a macro and may evaluate `stream` more than once.

### Parameters

- **stream** — to read the wide character from

### Return value

The next wide character from the stream or `WEOF` if an error has occurred or
the end of file has been reached. If an encoding error occurred, `errno` is set
to `EILSEQ`.

### See also

- **fgetcgetc** — gets a character from a file stream (function)
- **fgetws** — gets a wide string from a file stream (function)
- **fputwcputwc** — writes a wide character to a file stream (function)
- **ungetwc** — puts a wide character back into a file stream (function)

**C documentation for `fgetwc`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/fgetwc*
