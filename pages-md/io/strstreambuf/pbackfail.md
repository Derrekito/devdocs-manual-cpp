# std::strstreambuf::pbackfail

```cpp
protected:
virtual int_type pbackfail( int_type c = EOF );
```

This protected virtual function is called by the public functions
`basic_streambuf::sungetc` and `basic_streambuf::sputbackc` (which, in turn, are
called by `basic_istream::unget` and `basic_istream::putback`).

1) The caller is requesting that the get area is backed up by one character
   (`pbackfail()` is called with no arguments or with `EOF` as the argument)

   a) First, checks if there is a putback position, and if there really isn't,
      fails. (strstreambuf has no external character source to re-read)

   b) If the caller was wrong and the putback position is in fact available,
      simply decrements `basic_streambuf::gptr()`, e.g. by calling `gbump(-1)`.

2) The caller attempts to putback a different character from the one retrieved
   earlier (`pbackfail()` is called with the character that needs to be put
   back), in which case

   a) First, checks if there is a putback position, and if there isn't, fails.

   b) Then checks what character is in the putback position. If the character
      held there is already equal to `(char)c`, then simply decrements
      `basic_streambuf::gptr()`.

   c) Otherwise, if the buffer is unmodifiable (this strstreambuf was
      constructed with a string literal or some other `const` array), fails.

   d) Otherwise, decrements `basic_streambuf::gptr()` and writes `c` to the
      location pointed to gptr() after adjustment.

### Parameters

- **c** — the character to put back, or Traits::eof() to indicate that backing
  up of the get area is requested

### Return value

`c` on success except if `c` was `EOF`, in which case unspecified value other
than `EOF` is returned.

`EOF` on failure.

### Example

### See also

- **pbackfail [virtual]** — puts a character back into the input sequence,
  possibly modifying the input sequence (virtual protected member function of
  `std::basic_streambuf<CharT,Traits>`)
- **sungetc** — moves the next pointer in the input sequence back by one (public
  member function of `std::basic_streambuf<CharT,Traits>`)
- **sputbackc** — puts one character back in the input sequence (public member
  function of `std::basic_streambuf<CharT,Traits>`)
- **unget** — unextracts a character (public member function of
  `std::basic_istream<CharT,Traits>`)
- **putback** — puts a character into input stream (public member function of
  `std::basic_istream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/strstreambuf/pbackfail*
