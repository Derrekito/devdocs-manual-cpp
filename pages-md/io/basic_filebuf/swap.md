# std::basic_filebuf<CharT,Traits>::swap

```cpp
void swap( std::basic_filebuf& rhs );  // (since C++11)
```

Swaps the state and the contents of `*this` and `rhs`.

### Parameters

- **rhs** — another `basic_filebuf`

### Return value

(none)

### Notes

This function is called automatically when swapping `std::fstream` objects, it
is rarely necessary to call it directly.

### Example

```cpp
#include <fstream>
#include <iostream>
#include <string>

int main()
{
    std::ifstream fin("test.in"); // read-only
    std::ofstream fout("test.out"); // write-only

    std::string s;
    getline(fin, s);
    std::cout << s << '\n'; // outputs the first line of test.in

    fin.rdbuf()->swap(*fout.rdbuf()); //swap the underlying buffers

    getline(fin, s); // fails: cannot read from a write-only filebuf
    std::cout << s << '\n'; // prints empty line
}
```

### See also

- **operator= (C++11)** — assigns a `basic_filebuf` object (public member
  function)
- **std::swap(std::basic_filebuf) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **swap (C++11)** — swaps two file streams (public member function of
  `std::basic_fstream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_filebuf/swap*
