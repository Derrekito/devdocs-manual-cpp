# std::basic_spanbuf<CharT,Traits>::seekpos

```cpp
protected:
pos_type seekpos( pos_type sp, std::ios_base::openmode which =
                      std::ios_base::in | std::ios_base::out ) override;  // (since C++23)
```

Repositions the next pointer to the get and/or put area, if possible, to the
position indicated by `sp`.

Equivalent to `return seekoff(off_type(sp), std::ios_base::beg, which);`.

### Parameters

- **sp** — stream position, such as one obtained by `seekoff()` or `seekpos()`
- **which** — defines whether the input sequences, the output sequence, or both
  are affected. It can be one or a combination of the following constants:
  Constant Explanation `in` affect the input sequence `out` affect the output
  sequence
- **`in`** — affect the input sequence
- **`out`** — affect the output sequence

### Return value

`sp` on success or `pos_type(off_type(-1))` on failure.

### Notes

`seekpos()` is called by `std::basic_streambuf::pubseekpos()`, which is called
by the single-argument versions of `std::basic_istream::seekg()` and
`std::basic_ostream::seekp()`.

### Example

### See also

- **pubseekpos** — invokes `seekpos()` (public member function of
  `std::basic_streambuf<CharT,Traits>`)
- **seekpos [virtual]** — repositions the next pointer in the input sequence,
  output sequence, or both using absolute addressing (virtual protected member
  function of `std::basic_stringbuf<CharT,Traits,Allocator>`)
- **seekpos [virtual]** — repositions the next pointer in the input sequence,
  output sequence, or both using absolute addressing (virtual protected member
  function of `std::strstreambuf`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_spanbuf/seekpos*
