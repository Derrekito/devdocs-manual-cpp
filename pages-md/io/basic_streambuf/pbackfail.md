# std::basic_streambuf<CharT,Traits>::pbackfail

```cpp
protected:
virtual int_type pbackfail( int_type c = Traits::eof() );
```

This function can only be called if any of the following condition is satisfied:

- `gptr()` is null,
- `gptr() == eback()`, or
- `traits::eq(traits::to_char_type(c), gptr()[-1])` returns `false`.

This function is called by the public functions `sungetc()` and `sputbackc()`
(which, in turn, are called by `basic_istream::unget` and
`basic_istream::putback`) when either:

1) There is no putback position in the get area (`pbackfail()` is called with no
   arguments). In this situation, the purpose of `pbackfail()` is to back up the
   get area by one character, if the associated character sequence allows this
   (e.g. a file-backed streambuf may reload the buffer from a file, starting one
   character earlier).

2) The caller attempts to putback a different character from the one retrieved
   earlier (`pbackfail()` is called with the character that needs to be put
   back). In this situation, the purpose of `pbackfail()` is to place the
   character `c` into the get area at the position just before
   `basic_streambuf::gptr()`, and, if possible, to modify the associated
   character sequence to reflect this change. This may involve backing up the
   get area as in the first variant.

The default base class version of this function does nothing and returns
`Traits::eof()` in all situations. This function is overridden by the derived
classes: `basic_stringbuf::pbackfail`, `basic_filebuf::pbackfail`,
`strstreambuf::pbackfail`, and is expected to be overridden by user-defined and
third-party library stream classes.

### Parameters

- **ch** — character to put back or `Traits::eof()` if only back out is
  requested

### Return value

`Traits::eof()` in case of failure, some other value to indicate success. The
base class version always fails.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 32 | C++98 | the calling condition
      '`traits::eq(*gptr(),traits::to_char_type(c))` returns `false`' did not
      match the description of `sputbackc()` | corrected to match the
      description

### See also

- **pbackfail [virtual]** — backs out the input sequence to unget a character,
  not affecting the associated file (virtual protected member function of
  `std::basic_filebuf<CharT,Traits>`)
- **pbackfail [virtual]** — puts a character back into the input sequence
  (virtual protected member function of
  `std::basic_stringbuf<CharT,Traits,Allocator>`)
- **pbackfail [virtual]** — backs out the input sequence to unget a character
  (virtual protected member function of `std::strstreambuf`)
- **sungetc** — moves the next pointer in the input sequence back by one (public
  member function)
- **sputbackc** — puts one character back in the input sequence (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/pbackfail*
