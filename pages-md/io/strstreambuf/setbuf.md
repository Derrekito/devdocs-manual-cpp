# std::strstreambuf::setbuf

```cpp
protected:
virtual streambuf* setbuf( char* s, std::streamsize n );
```

If `s` is a null pointer and `n` is zero, this function has no effect.

Otherwise, the effect is implementation-defined: some implementations do
nothing, while some implementations deallocate the dynamic member array used as
the buffer and begin using the user-supplied character array of size `n`, whose
first element is pointed to by `s`.

This function is protected virtual, it may only be called through `pubsetbuf()`
or from member functions of a user-defined class derived from
`std::strstreambuf`.

### Parameters

- **s** — pointer to the first byte in the user-provided buffer
- **n** — the number of bytes in the user-provided buffer

### Return value

`this`

### Example

Implementation test to check if `setbuf()` is supported on a dynamic strstream
(output obtained with Sun Studio):

```cpp
#include <iostream>
#include <strstream>

int main()
{
    char a[100] = {};
    std::strstream str;
    str.rdbuf()->pubsetbuf(a, sizeof a);
    str << "Test string" << std::ends;
    std::cout << "user-provided buffer holds \"" << a << "\"\n";
}
```

Possible output:

```text
user-provided buffer holds "Test string"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 66 | C++98 | the effect of `setbuf()` was "performs an operation that is
      defined separately for each class derived from `strstreambuf`", but there
      are no classes derived from `strstreambuf` | the effect is
      implementation-defined

### See also

- **pubsetbuf** — invokes `setbuf()` (public member function of
  `std::basic_streambuf<CharT,Traits>`)
- **setbuf [virtual]** — replaces the buffer with user-defined array, if
  permitted (virtual protected member function of
  `std::basic_streambuf<CharT,Traits>`)
- **setbuf [virtual]** — attempts to replace the controlled character sequence
  with an array (virtual protected member function of
  `std::basic_stringbuf<CharT,Traits,Allocator>`)
- **setbuf [virtual]** — provides user-supplied buffer or turns this filebuf
  unbuffered (virtual protected member function of
  `std::basic_filebuf<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/strstreambuf/setbuf*
