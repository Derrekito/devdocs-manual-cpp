# std::fgetpos

```cpp
int fgetpos( std::FILE* stream, std::fpos_t* pos );
```

Obtains the file position indicator and the current parse state (if any) for the
file stream `stream` and stores them in the object pointed to by `pos`. The
value stored is only meaningful as the input to `std::fsetpos`.

### Parameters

- **stream** — file stream to examine
- **pos** — pointer to a `fpos_t` object to store the file position indicator to

### Return value

`​0​` upon success, nonzero value otherwise. Also sets `errno` on failure.

### Example

### See also

- **ftell** — returns the current file position indicator (function)
- **fseek** — moves the file position indicator to a specific location in a file
  (function)
- **fsetpos** — moves the file position indicator to a specific location in a
  file (function)

**C documentation for `fgetpos`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/fgetpos*
