# std::basic_stringbuf<CharT,Traits,Allocator>::seekpos

```cpp
protected:
virtual pos_type seekpos( pos_type sp,
                          std::ios_base::openmode which = std::ios_base::in | std::ios_base::out );
```

Repositions `std::basic_streambuf::gptr` and/or `std::basic_streambuf::pptr`, if
possible, to the position indicated by `sp`.

Effectively executes `seekoff(off_type(sp), std::ios_base::beg, which)`.

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

```cpp
#include <sstream>
#include <iostream>

struct mybuf : std::stringbuf
{
    mybuf(const std::string& str) : std::stringbuf(str) {}

    pos_type seekpos(pos_type sp, std::ios_base::openmode which)
    {
        std::cout << "Before seekpos(" << sp << "), size of the get area is "
                  << egptr() - eback() << " with "
                  << egptr() - gptr() << " read positions available.\n";

        pos_type rc = std::stringbuf::seekpos(sp, which);

        std::cout << "seekpos() returns " << rc << ".\nAfter the call, "
                  << "size of the get area is "
                  << egptr() - eback() << " with "
                  << egptr() - gptr() << " read positions available.\n";

        return rc;
    }
};

int main()
{
    mybuf buf("12345");
    std::iostream stream(&buf);
    stream.seekg(2);
}
```

Output:

```text
Before seekpos(2), size of the get area is 5 with 5 read positions available.
seekpos() returns 2.
After the call, size of the get area is 5 with 3 read positions available.
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 375 | C++98 | static constant members of `std::ios_base` were misspecified
      as members of `std::basic_ios` | corrected
  LWG 564 | C++98 | if was unclear how to reposition `gptr` and/or `pptr` | they
      are repositioned by `seekoff()`

### See also

- **pubseekpos** — invokes `seekpos()` (public member function of
  `std::basic_streambuf<CharT,Traits>`)
- **seekoff [virtual]** — repositions the next pointer in the input sequence,
  output sequence, or both, using relative addressing (virtual protected member
  function)
- **seekpos [virtual]** — repositions the file position, using absolute
  addressing (virtual protected member function of
  `std::basic_filebuf<CharT,Traits>`)
- **seekpos [virtual]** — repositions the next pointer in the input sequence,
  output sequence, or both using absolute addressing (virtual protected member
  function of `std::strstreambuf`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_stringbuf/seekpos*
