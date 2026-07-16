# std::strstreambuf::underflow

```cpp
protected:
virtual int_type underflow();
```

Reads the next character from the get area of the buffer.

If the input sequence has a read position available (`gptr() < egptr()`, returns
`(unsigned char)(*gptr())`.

Otherwise, if `pptr()` is not null and `pptr() > egptr()` (there is a put area
and it is located after the get area), extends the end of the get area to
include the characters that were recently written into the put area by
incrementing `egptr()` to some value between `gptr()` and `pptr()`, and then
returns `(unsigned char)(*gptr())`.

Otherwise, returns `EOF` to indicate failure.

### Parameters

(none)

### Return value

The next character in the get area, `(unsigned char)(*gptr())` on success, `EOF`
on failure.

### Example

```cpp
#include <iostream>
#include <strstream>

struct mybuf : std::strstreambuf
{
    int_type overflow(int_type c)
    {
        std::cout << "Before overflow(): size of the get area is " << egptr()-eback()
                  << " size of the put area is " << epptr()-pbase() << '\n';
        int_type rc = std::strstreambuf::overflow(c);
        std::cout << "After overflow(): size of the get area is " << egptr()-eback()
                  << " size of the put area is " << epptr()-pbase() << '\n';
        return rc;
    }

    int_type underflow()
    {
        std::cout << "Before underflow(): size of the get area is " << egptr()-eback()
                  << " size of the put area is " << epptr()-pbase() << '\n';
        int_type ch = std::strstreambuf::underflow();
        std::cout << "After underflow(): size of the get area is " << egptr()-eback()
                  << " size of the put area is " << epptr()-pbase() << '\n';
        if (ch == EOF)
            std::cout << "underflow() returns EOF\n";
        else
            std::cout << "underflow() returns '" << char(ch) << "'\n";
        return ch;
    }
};

int main()
{
    mybuf sbuf; // read-write dynamic strstreambuf
    std::iostream stream(&sbuf);

    int n;
    stream >> n;
    stream.clear();
    stream << "123";
    stream >> n;
    std::cout << n << '\n';
}
```

Possible output:

```text
Before underflow(): size of the get area is 0 size of the put area is 0
After underflow(): size of the get area is 0 size of the put area is 0
underflow() returns EOF
Before overflow(): size of the get area is 0 size of the put area is 0
After overflow(): size of the get area is 0 size of the put area is 32
Before underflow(): size of the get area is 0 size of the put area is 32
After underflow(): size of the get area is 3 size of the put area is 32
underflow() returns '1'
Before underflow(): size of the get area is 3 size of the put area is 32
After underflow(): size of the get area is 3 size of the put area is 32
underflow() returns EOF
123
```

### See also

- **underflow [virtual]** — reads characters from the associated input sequence
  to the get area (virtual protected member function of
  `std::basic_streambuf<CharT,Traits>`)
- **underflow [virtual]** — returns the next character available in the input
  sequence (virtual protected member function of
  `std::basic_stringbuf<CharT,Traits,Allocator>`)
- **underflow [virtual]** — reads from the associated file (virtual protected
  member function of `std::basic_filebuf<CharT,Traits>`)
- **sgetc** — reads one character from the input sequence without advancing the
  sequence (public member function of `std::basic_streambuf<CharT,Traits>`)
- **get** — extracts characters (public member function of
  `std::basic_istream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/strstreambuf/underflow*
