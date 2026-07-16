# std::basic_streambuf<CharT,Traits>::pubsetbuf, std::basic_streambuf<CharT,Traits>::setbuf

```cpp
public:
basic_streambuf<CharT, Traits>* pubsetbuf( char_type* s, std::streamsize n )  // (1)
protected:
virtual basic_streambuf<CharT, Traits>* setbuf( char_type* s, std::streamsize n )  // (2)
```

1) Calls `setbuf(s, n)` of the most derived class.

2) The base class version of this function has no effect. The derived classes
   may override this function to allow removal or replacement of the controlled
   character sequence (the buffer) with a user-provided array, or for any other
   implementation-specific purpose.

### Parameters

- **s** — pointer to the first `CharT` in the user-provided buffer
- **n** — the number of `CharT` elements in the user-provided buffer

### Return value

1) The return value of `setbuf(s, n)`.

2) `this`

### Example

Provides a 10k buffer for reading. On linux, the strace utility may be used to
observe the actual number of bytes read.

```cpp
#include <fstream>
#include <iostream>
#include <string>

int main()
{
    int cnt = 0;
    std::ifstream file;
    char buf[1024 * 10 + 1];

    file.rdbuf()->pubsetbuf(buf, sizeof buf);

    file.open("/usr/share/dict/words");

    for (std::string line; getline(file, line);)
        ++cnt;

    std::cout << cnt << '\n';
}
```

Possible output:

```text
356010
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 158 | C++98 | the default behavior of `setbuf` was only specified if
      `gptr()` is not null and not equal to `egptr()` | specified as no-op for
      all cases

### See also

- **setbuf [virtual]** — attempts to replace the controlled character sequence
  with an array (virtual protected member function of
  `std::basic_stringbuf<CharT,Traits,Allocator>`)
- **setbuf [virtual]** — provides user-supplied buffer or turns this filebuf
  unbuffered (virtual protected member function of
  `std::basic_filebuf<CharT,Traits>`)
- **setbuf [virtual]** — attempts to replace the controlled character sequence
  with an array (virtual protected member function of `std::strstreambuf`)
- **setbuf** — sets the buffer for a file stream (function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/pubsetbuf*
