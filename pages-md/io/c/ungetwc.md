# std::ungetwc

```cpp
std::wint_t ungetwc( std::wint_t ch, std::FILE* stream );
```

If `ch` does not equal `WEOF`, pushes the wide character `ch` into the input
buffer associated with the stream `stream` in such a manner than subsequent read
operation from `stream` will retrieve that wide character. The external device
associated with the stream is not modified.

Stream repositioning operations `std::fseek`, `std::fsetpos`, and `std::rewind`
discard the effects of `std::ungetwc`.

If `std::ungetwc` is called more than once without an intervening read or
repositioning, it may fail (in other words, a pushback buffer of size 1 is
guaranteed, but any larger buffer is implementation-defined). If multiple
successful `std::ungetwc` were performed, read operations retrieve the
pushed-back wide characters in reverse order of `std::ungetwc`

If `ch` equals `WEOF`, the operation fails and the stream is not affected.

A successful call to `std::ungetwc` clears the end of file status flag `feof`.

A successful call to `std::ungetwc` on a stream (whether text or binary)
modifies the stream position indicator in unspecified manner but guarantees that
after all pushed-back wide characters are retrieved with a read operation, the
stream position indicator is equal to its value before `std::ungetwc`.

### Parameters

- **ch** — wide character to be put back
- **stream** — file stream to put the wide character back to

### Return value

On success `ch` is returned.

On failure `WEOF` is returned and the given stream remains unchanged.

### See also

- **ungetc** — puts a character back into a file stream (function)
- **fgetwcgetwc** — gets a wide character from a file stream (function)

**C documentation for `ungetwc`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/ungetwc*
