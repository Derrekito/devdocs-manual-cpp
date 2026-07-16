# std::basic_syncbuf<CharT,Traits,Allocator>::operator=

```cpp
basic_syncbuf& operator=( basic_syncbuf&& other );
```

First, calls `emit()` to transmit all pending output (and delayed flush, if any)
to the wrapped stream.

Then performs move-assignment by moving all contents from `other`, including the
temporary storage, the wrapped stream pointer, policy, and all other state (such
as the mutex pointer). After move, `other` is not associated with a stream, and
`other.get_wrapped() == nullptr`. The put area member pointers of the base class
`std::basic_streambuf` of `other` are guaranteed to be null. Destroying a
moved-from `other` will not produce any output.

If
`std::allocator_traits<Allocator>::propagate_on_container_move_assignment::value`
is `false`, then the allocator is unchanged. Otherwise, after move-assignment,
`get_allocator()` equals `other.get_allocator()`.

### Parameters

- **other** — another std::basic_syncbuf to move from

### Return value

`*this`

### Example

### See also

- **operator=** — assigns a `basic_osyncstream` object (public member function
  of `std::basic_osyncstream<CharT,Traits,Allocator>`)
- **(constructor)** — constructs a `basic_syncbuf` object (public member
  function)
- **emit** — atomically transmits the entire internal buffer to the wrapped
  streambuf (public member function)
- **swap** — swaps two `basic_syncbuf` objects (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_syncbuf/operator%3D*
