# std::basic_spanbuf

```cpp
template<
    class CharT,
    class Traits = std::char_traits<CharT>
> class basic_spanbuf
    : public std::basic_streambuf<CharT, Traits>  // (since C++23)
```

`std::basic_spanbuf` is a `std::basic_streambuf` whose associated character
sequence is a memory-resident sequence of arbitrary characters, which can be
initialized from or made available as an instance of std::span<CharT>.

`std::basic_spanbuf` performs I/O on a fixed buffer, and therefore it does not
attempt to obtain a new buffer when the underlying buffer is exhausted.

Several typedefs for common character types are provided:

- **`std::spanbuf`** — std::basic_spanbuf<char>
- **`std::wspanbuf`** — std::basic_spanbuf<wchar_t>

### Member types

- **`char_type`** — `CharT`
- **`traits_type`** — `Traits`; the program is ill-formed if `Traits::char_type`
  is not `CharT`.
- **`int_type`** — `Traits::int_type`
- **`pos_type`** — `Traits::pos_type`
- **`off_type`** — `Traits::off_type`

### Data members

- **`mod` (private)** — A `std::ios_base::openmode` recording the open mode.
  (exposition-only member object*)
- **`buf` (private)** — A std::span<CharT> referencing the underlying buffer.
  (exposition-only member object*)

### Member functions

**Public member functions**

- **(constructor) (C++23)** — constructs a `basic_spanbuf` object (public member
  function)
- ****(destructor)** [virtual] (C++23)** — destroys the `basic_spanbuf` object
  (virtual public member function)
- **operator= (C++23)** — assigns a `basic_spanbuf` object (public member
  function)
- **swap (C++23)** — swaps two `basic_spanbuf` objects (public member function)
- **span (C++23)** — obtains or initializes an underlying buffer according to
  mode (public member function)

**Protected member functions**

- **setbuf [virtual] (C++23)** — attempts to replace the controlled character
  sequence with an array (virtual protected member function)
- **seekoff [virtual] (C++23)** — repositions the next pointer in the input
  sequence, output sequence, or both, using relative addressing (virtual
  protected member function)
- **seekpos [virtual] (C++23)** — repositions the next pointer in the input
  sequence, output sequence, or both using absolute addressing (virtual
  protected member function)

### Non-member functions

- **std::swap(std::basic_spanbuf) (C++23)** — specializes the `std::swap`
  algorithm (function template)

## Inherited from std::basic_streambuf

### Member types

- **`char_type`** — `CharT`
- **`traits_type`** — `Traits`; the program is ill-formed if `Traits::char_type`
  is not `CharT`.
- **`int_type`** — `Traits::int_type`
- **`pos_type`** — `Traits::pos_type`
- **`off_type`** — `Traits::off_type`

### Member functions

- **(destructor) [virtual]** — destructs the `basic_streambuf` object (virtual
  public member function of `std::basic_streambuf<CharT,Traits>`)

**Locales**

- **pubimbue** — invokes `imbue()` (public member function of
  `std::basic_streambuf<CharT,Traits>`)
- **getloc** — obtains a copy of the associated locale (public member function
  of `std::basic_streambuf<CharT,Traits>`)

**Positioning**

- **pubsetbuf** — invokes `setbuf()` (public member function of
  `std::basic_streambuf<CharT,Traits>`)
- **pubseekoff** — invokes `seekoff()` (public member function of
  `std::basic_streambuf<CharT,Traits>`)
- **pubseekpos** — invokes `seekpos()` (public member function of
  `std::basic_streambuf<CharT,Traits>`)
- **pubsync** — invokes `sync()` (public member function of
  `std::basic_streambuf<CharT,Traits>`)

**Get area**

- **in_avail** — obtains the number of characters immediately available in the
  get area (public member function of `std::basic_streambuf<CharT,Traits>`)
- **snextc** — advances the input sequence, then reads one character without
  advancing again (public member function of
  `std::basic_streambuf<CharT,Traits>`)
- **sbumpcstossc (removed in C++17)** — reads one character from the input
  sequence and advances the sequence (public member function of
  `std::basic_streambuf<CharT,Traits>`)
- **sgetc** — reads one character from the input sequence without advancing the
  sequence (public member function of `std::basic_streambuf<CharT,Traits>`)
- **sgetn** — invokes `xsgetn()` (public member function of
  `std::basic_streambuf<CharT,Traits>`)

**Put area**

- **sputc** — writes one character to the put area and advances the next pointer
  (public member function of `std::basic_streambuf<CharT,Traits>`)
- **sputn** — invokes `xsputn()` (public member function of
  `std::basic_streambuf<CharT,Traits>`)

**Putback**

- **sputbackc** — puts one character back in the input sequence (public member
  function of `std::basic_streambuf<CharT,Traits>`)
- **sungetc** — moves the next pointer in the input sequence back by one (public
  member function of `std::basic_streambuf<CharT,Traits>`)

### Protected member functions

- **(constructor)** — constructs a `basic_streambuf` object (protected member
  function)
- **operator= (C++11)** — replaces a `basic_streambuf` object (protected member
  function)
- **swap (C++11)** — swaps two `basic_streambuf` objects (protected member
  function)

**Locales**

- **imbue [virtual]** — changes the associated locale (virtual protected member
  function of `std::basic_streambuf<CharT,Traits>`)

**Positioning**

- **setbuf [virtual]** — replaces the buffer with user-defined array, if
  permitted (virtual protected member function of
  `std::basic_streambuf<CharT,Traits>`)
- **seekoff [virtual]** — repositions the next pointer in the input sequence,
  output sequence, or both, using relative addressing (virtual protected member
  function of `std::basic_streambuf<CharT,Traits>`)
- **seekpos [virtual]** — repositions the next pointer in the input sequence,
  output sequence, or both using absolute addressing (virtual protected member
  function of `std::basic_streambuf<CharT,Traits>`)
- **sync [virtual]** — synchronizes the buffers with the associated character
  sequence (virtual protected member function of
  `std::basic_streambuf<CharT,Traits>`)

**Get area**

- **showmanyc [virtual]** — obtains the number of characters available for input
  in the associated input sequence, if known (virtual protected member function
  of `std::basic_streambuf<CharT,Traits>`)
- **underflow [virtual]** — reads characters from the associated input sequence
  to the get area (virtual protected member function of
  `std::basic_streambuf<CharT,Traits>`)
- **uflow [virtual]** — reads characters from the associated input sequence to
  the get area and advances the next pointer (virtual protected member function
  of `std::basic_streambuf<CharT,Traits>`)
- **xsgetn [virtual]** — reads multiple characters from the input sequence
  (virtual protected member function of `std::basic_streambuf<CharT,Traits>`)
- **ebackgptregptr** — returns a pointer to the beginning, current character and
  the end of the get area (protected member function)
- **gbump** — advances the next pointer in the input sequence (protected member
  function)
- **setg** — repositions the beginning, next, and end pointers of the input
  sequence (protected member function)

**Put area**

- **xsputn [virtual]** — writes multiple characters to the output sequence
  (virtual protected member function of `std::basic_streambuf<CharT,Traits>`)
- **overflow [virtual]** — writes characters to the associated output sequence
  from the put area (virtual protected member function of
  `std::basic_streambuf<CharT,Traits>`)
- **pbasepptrepptr** — returns a pointer to the beginning, current character and
  the end of the put area (protected member function)
- **pbump** — advances the next pointer of the output sequence (protected member
  function)
- **setp** — repositions the beginning, next, and end pointers of the output
  sequence (protected member function)

**Putback**

- **pbackfail [virtual]** — puts a character back into the input sequence,
  possibly modifying the input sequence (virtual protected member function of
  `std::basic_streambuf<CharT,Traits>`)

### Notes

`std::basic_spanbuf` does not own the underlying buffer.

It is the responsibility of programmers to ensure the underlying buffer is in
its lifetime when used by a `std::basic_spanbuf` object. Additional
synchronization may be needed if more than one thread operates the same
underlying buffer through different `std::basic_spanbuf` objects.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_spanstream` | 202106L | (C++23) | `std::spanbuf`, `std::spanstream`

### See also

- **basic_stringbuf** — implements raw string device (class template)
- **strstreambuf (deprecated in C++98)** — implements raw character array device
  (class)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_spanbuf*
