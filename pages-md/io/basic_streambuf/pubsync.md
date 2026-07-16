# std::basic_streambuf<CharT,Traits>::pubsync, std::basic_streambuf<CharT,Traits>::sync

```cpp
int pubsync();  // (1)
protected:
virtual int sync();  // (2)
```

Synchronizes the controlled character sequence (the buffers) with the associated
character sequence.

1) Calls `sync()` of the most derived class

2) The base class version of this function has no effect. The derived classes
may override this function to allow synchronizing the underlying device with the
buffers.

For output streams, this typically results in writing the contents of the put
area into the associated sequence, i.e. flushing of the output buffer. For input
streams, this typically empties the get area and forces a re-read from the
associated sequence to pick up recent changes. The default behavior (found, for
example, in `std::basic_stringbuf`), is to do nothing.

### Parameters

(none)

### Return value

1) The return value of `sync()`.

2) Returns `​0​` on success, `-1` otherwise. The base class version returns
`​0​`.

### Example

### See also

- **sync** — synchronizes with the underlying storage device (public member
  function of `std::basic_istream<CharT,Traits>`)
- **sync [virtual]** — writes characters to the associated file from the put
  area (virtual protected member function of `std::basic_filebuf<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/pubsync*
