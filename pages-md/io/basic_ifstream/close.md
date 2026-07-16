# std::basic_ifstream<CharT,Traits>::close

```cpp
void close();
```

Closes the associated file.

Effectively calls `rdbuf()->close()`. If an error occurs during operation,
`setstate(failbit)` is called.

### Parameters

(none)

### Return value

(none)

### Notes

This function is called by the destructor of `basic_ifstream` when the stream
object goes out of scope and is not usually invoked directly.

### Example

### See also

- **is_open** — checks if the stream has an associated file (public member
  function)
- **open** — opens a file and associates it with the stream (public member
  function)
- **close** — flushes the put area buffer and closes the associated file (public
  member function of `std::basic_filebuf<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ifstream/close*
