# std::basic_filebuf<CharT,Traits>::showmanyc

```cpp
protected:
virtual std::streamsize showmanyc()
```

If implemented, returns the number of characters left to read from the file.

### Parameters

(none)

### Return value

The number of characters available for reading from the file, or `-1` if the end
of file was reached.

### Notes

This function is optional. If not implemented, this function returns `​0​`
(since the base class version `std::basic_streambuf::showmanyc` gets called)

Whether implemented or not, this function is normally called by
`std::basic_streambuf::in_avail` if the get area is empty.

The name of this function stands for "stream: how many characters?", so it is
pronounced "S how many C", rather than "show many C"

### Example

implementation test to see if showmanyc() is implemented for filebuf

```cpp
#include <fstream>
#include <iostream>

struct mybuf : std::filebuf
{
     using std::filebuf::showmanyc;
};

int main()
{
    mybuf fin;
    fin.open("main.cpp", std::ios_base::in);
    std::cout << "showmanyc() returns " << fin.showmanyc() << '\n';
}
```

Possible output:

```text
showmanyc() returns 267
```

### See also

- **in_avail** — obtains the number of characters immediately available in the
  get area (public member function of `std::basic_streambuf<CharT,Traits>`)
- **readsome** — extracts already available blocks of characters (public member
  function of `std::basic_istream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_filebuf/showmanyc*
