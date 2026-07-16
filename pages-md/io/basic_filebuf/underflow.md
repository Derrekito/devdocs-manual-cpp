# std::basic_filebuf<CharT,Traits>::underflow

```cpp
protected:
virtual int_type underflow()
```

Reads more data into the input area.

Behaves like the base class `std::basic_streambuf::underflow`, except that to
read the data from the associated character sequence (the file) into the get
area, first reads the bytes from the file into a temporary buffer (allocated as
large as necessary), then uses `std::codecvt::in` of the imbued locale to
convert the external (typically, multibyte) representation to the internal form
which is then used to populate the get area. The conversion may be skipped if
the locale's `std::codecvt::always_noconv` returns `true`.

### Parameters

(none)

### Return value

`Traits::to_int_type(*gptr())` (the first character of the pending sequence) in
case of success, or `Traits::eof()` in case of failure.

### Example

```cpp
#include <fstream>
#include <iostream>

struct mybuf : std::filebuf
{
    int underflow()
    {
         std::cout << "Before underflow(): size of the get area is "
                   << egptr()-eback() << " with "
                   << egptr()-gptr() << " read positions available\n";
         int rc = std::filebuf::underflow();
         std::cout << "underflow() returns " << rc << ".\nAfter the call, "
                   << "size of the get area is "
                   << egptr()-eback() << " with "
                   << egptr()-gptr() << " read positions available\n";
        return rc;
    }
};

int main()
{
    mybuf buf;
    buf.open("test.txt", std::ios_base::in);
    std::istream stream(&buf);
    while (stream.get()) ;
}
```

Possible output:

```text
Before underflow(): size of the get area is 0 with 0 read positions available
underflow() returns 73.
After the call, size of the get area is 110 with 110 read positions available
Before underflow(): size of the get area is 110 with 0 read positions available
underflow() returns -1.
After the call, size of the get area is 0 with 0 read positions available
```

### See also

- **underflow [virtual]** — reads characters from the associated input sequence
  to the get area (virtual protected member function of
  `std::basic_streambuf<CharT,Traits>`)
- **underflow [virtual]** — returns the next character available in the input
  sequence (virtual protected member function of
  `std::basic_stringbuf<CharT,Traits,Allocator>`)
- **underflow [virtual]** — reads a character from the input sequence without
  advancing the next pointer (virtual protected member function of
  `std::strstreambuf`)
- **uflow [virtual]** — reads from the associated file and advances the next
  pointer in the get area (virtual protected member function)
- **overflow [virtual]** — writes characters to the associated file from the put
  area (virtual protected member function)
- **sgetc** — reads one character from the input sequence without advancing the
  sequence (public member function of `std::basic_streambuf<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_filebuf/underflow*
