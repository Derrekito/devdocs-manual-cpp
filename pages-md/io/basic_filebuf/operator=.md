# std::basic_filebuf<CharT,Traits>::operator=

```cpp
std::basic_filebuf& operator=( std::basic_filebuf&& rhs );  // (1) (since C++11)
std::basic_filebuf& operator=( const std::basic_filebuf& rhs ) = delete;  // (2)
```

Assigns another `basic_filebuf` object.

1) First calls `close()` to close the associated file, then moves the contents
   of `rhs` into `*this`: the put and get buffers, the associated file, the
   locale, the openmode, the is_open flag, and any other state. After the move,
   `rhs` is not associated with a file and `rhs.is_open() == false`.

2) The copy assignment operator is deleted; `basic_filebuf` is not
   CopyAssignable.

### Parameters

- **rhs** — another `basic_filebuf` that will be moved from

### Return value

`*this`

### Example

```cpp
#include <cassert>
#include <fstream>
#include <iostream>
#include <string>

int main()
{
    std::ofstream{"test.in"} << "test\n"; // writes via a temporary object
    std::ifstream fin("test.in"); // read-only stream
    std::ofstream fout("test.out"); // write-only stream

    std::string s;
    std::getline(fin, s);
    std::cout << "s = [" << s << "]\n"; // s contains "test"

    assert(fout.is_open());
    *fin.rdbuf() = std::move(*fout.rdbuf());
    assert(!fout.is_open());

    std::getline(fin, s);
    std::cout << "s = [" << s << "]\n"; // s is empty input
}
```

Output:

```text
s = [test]
s = []
```

### See also

- **(constructor)** — constructs a `basic_filebuf` object (public member
  function)
- **swap (C++11)** — swaps two `basic_filebuf` objects (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_filebuf/operator%3D*
