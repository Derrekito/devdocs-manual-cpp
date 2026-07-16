# std::basic_streambuf<CharT,Traits>::showmanyc

```cpp
protected:
virtual std::streamsize showmanyc();
```

Estimates the number of characters available for input in the associated
character sequence. `underflow()` is guaranteed not to return `Traits::eof()` or
throw an exception until at least that many characters are extracted.

### Parameters

(none)

### Return value

The number of characters that are certainly available in the associated
character sequence, or `-1` if `showmanyc` can determine, without blocking, that
no characters are available. If `showmanyc` returns `-1`, `underflow()` and
`uflow()` will definitely return `Traits::eof` or throw.

The base class version returns `​0​`, which has the meaning of "unsure if there
are characters available in the associated sequence".

### Notes

The name of this function stands for "stream: how many characters?", so it is
pronounced "S how many C", rather than "show many C".

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 159 | C++98 | `underflow()` is guaranteed not to fail until at least
      `showmanyc()` characters are supplied | extended the guarantee to allow at
      least `showmanyc()` characters to be extracted

### See also

- **in_avail** — obtains the number of characters immediately available in the
  get area (public member function)
- **showmanyc [virtual]** — optionally provides the number of characters
  available for input from the file (virtual protected member function of
  `std::basic_filebuf<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/showmanyc*
