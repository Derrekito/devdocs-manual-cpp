# std::basic_streambuf<CharT,Traits>::pubseekpos, std::basic_streambuf<CharT,Traits>::seekpos

```cpp
pos_type pubseekpos( pos_type pos,
                     std::ios_base::openmode which = std::ios_base::in | std::ios_base::out );  // (1)
protected:
virtual pos_type seekpos( pos_type pos,
                          std::ios_base::openmode which = std::ios_base::in | std::ios_base::out );  // (2)
```

Sets the position indicator of the input and/or output sequence to an absolute
position.

1) Calls `seekpos(pos, which)` of the most derived class.

2) The base class version of this function has no effect. The derived classes
   may override this function to allow absolute positioning of the position
   indicator.

### Parameters

- **pos** — absolute position to set the position indicator to
- **which** — defines which of the input and/or output sequences to affect. It
  can be one or a combination of the following constants: Constant Explanation
  `in` affect the input sequence `out` affect the output sequence
- **`in`** — affect the input sequence
- **`out`** — affect the output sequence

### Return value

1) The return value of `seekpos(pos, which)`.

2) The resulting absolute position as defined by the position indicator. The
   base class version returns `pos_type(off_type(-1))`.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 55 | C++98 | the base class version of `seekpos` returned an undefined
      invalid stream position | returns `pos_type(off_type(-1))`

### See also

- **pubseekoff** — invokes `seekoff()` (public member function)
- **seekpos [virtual]** — repositions the file position, using absolute
  addressing (virtual protected member function of
  `std::basic_filebuf<CharT,Traits>`)
- **seekpos [virtual]** — repositions the next pointer in the input sequence,
  output sequence, or both using absolute addressing (virtual protected member
  function of `std::basic_stringbuf<CharT,Traits,Allocator>`)
- **seekpos [virtual]** — repositions the next pointer in the input sequence,
  output sequence, or both using absolute addressing (virtual protected member
  function of `std::strstreambuf`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/pubseekpos*
