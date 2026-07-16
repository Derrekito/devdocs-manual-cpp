# std::basic_streambuf<CharT,Traits>::pubseekoff, std::basic_streambuf<CharT,Traits>::seekoff

```cpp
pos_type pubseekoff( off_type off, std::ios_base::seekdir dir,
                     std::ios_base::openmode which = ios_base::in | ios_base::out );  // (1)
protected:
virtual pos_type seekoff( off_type off, std::ios_base::seekdir dir,
                          std::ios_base::openmode which = ios_base::in | ios_base::out );  // (2)
```

Sets the position indicator of the input and/or output sequence relative to some
other position.

1) Calls `seekoff(off, dir, which)` of the most derived class.

2) The base class version of this function has no effect. The derived classes
   may override this function to allow relative positioning of the position
   indicator.

### Parameters

- **off** — relative position to set the position indicator to.
- **dir** — defines base position to apply the relative offset to. It can be one
  of the following constants: Constant Explanation `beg` the beginning of a
  stream `end` the ending of a stream `cur` the current position of stream
  position indicator
- **`beg`** — the beginning of a stream
- **`end`** — the ending of a stream
- **`cur`** — the current position of stream position indicator
- **which** — defines which of the input and/or output sequences to affect. It
  can be one or a combination of the following constants: Constant Explanation
  `in` affect the input sequence `out` affect the output sequence
- **`in`** — affect the input sequence
- **`out`** — affect the output sequence

### Note

Not all combinations of parameters may be valid, see the derived versions of
`seekoff` for details.

### Return value

1) The return value of `seekoff(off, dir, which)`

2) The resulting absolute position as defined by the position indicator. The
   base class version returns `pos_type(off_type(-1))`.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 55 | C++98 | the base class version of `seekoff` returned an undefined
      invalid stream position | returns `pos_type(off_type(-1))`

### See also

- **pubseekpos** — invokes `seekpos()` (public member function)
- **seekoff [virtual]** — repositions the file position, using relative
  addressing (virtual protected member function of
  `std::basic_filebuf<CharT,Traits>`)
- **seekoff [virtual]** — repositions the next pointer in the input sequence,
  output sequence, or both, using relative addressing (virtual protected member
  function of `std::basic_stringbuf<CharT,Traits,Allocator>`)
- **seekoff [virtual]** — repositions the next pointer in the input sequence,
  output sequence, or both, using relative addressing (virtual protected member
  function of `std::strstreambuf`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/pubseekoff*
