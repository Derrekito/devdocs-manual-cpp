# std::basic_syncbuf<CharT,Traits,Allocator>::basic_syncbuf

```cpp
basic_syncbuf()
    : basic_syncbuf( nullptr )  // (1)
explicit basic_syncbuf( streambuf_type* obuf )
    : basic_syncbuf( obuf, Allocator() ) {}  // (2)
basic_syncbuf( streambuf_type* obuf, const Allocator& a );  // (3)
basic_syncbuf( basic_syncbuf&& rhs );  // (4)
```

1) Default constructor: creates an instance of `std::basic_syncbuf` with
   emit-on-sync policy set to `false`, wrapped streambuffer set to `nullptr`,
   and using default-constructed `Allocator` as the allocator for temporary
   storage.

2,3) Creates an instance of `std::basic_syncbuf` with emit-on-sync policy set to
   `false`, wrapped streambuffer set to `obuf`, and using `a` as the allocator
   for temporary storage.

4) Move constructor: move-constructs a `std::basic_syncbuf` object by moving all
   contents from another `std::basic_syncbuf` object `rhs`, including the
   temporary storage, the wrapped stream pointer, policy, and all other state
   (such as the mutex pointer). After move, `rhs` is not associated with a
   stream, and `rhs.get_wrapped() == nullptr`. The put area member pointers of
   the base class `std::basic_streambuf` of `rhs` are guaranteed to be null.
   Destroying a moved-from `rhs` will not produce any output.

### Parameters

- **obuf** — pointer to the `std::basic_streambuf` to wrap
- **a** — the allocator to use for temporary storage
- **rhs** — another `std::basic_syncbuf` to move from

### Exceptions

2,3) May throw `std::bad_alloc` from the constructor of the internal temporary
   storage or `std::system_error` from the mutex construction.

### Notes

Typically called by the appropriate constructors of `std::basic_osyncstream`.

### Example

### See also

- **sync [virtual]** — synchronizes the buffers with the associated character
  sequence (virtual protected member function of
  `std::basic_streambuf<CharT,Traits>`)
- **emit** — atomically transmits the entire internal buffer to the wrapped
  streambuf (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_syncbuf/basic_syncbuf*
