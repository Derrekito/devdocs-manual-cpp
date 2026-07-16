# std::basic_istream<CharT,Traits>::~basic_istream

```cpp
virtual ~basic_istream();
```

Destructs the input stream.

### Notes

This destructor does not perform any operation on the underlying streambuffer
(`rdbuf()`): the destructors of the derived input streams such as
`std::basic_ifstream` and `std::basic_istringstream` are responsible for calling
the destructors of the streambuffers.

### Example

```cpp
#include <iostream>
#include <sstream>

void print_stringbuf(std::streambuf* p)
{
    std::istream buf(p); // buf shares the buffer with s1
    int n;
    buf >> n;
    std::cout << n;
} // calls the destructor of buf. p remains unaffected

int main()
{
    std::istringstream s1("10 20");
    print_stringbuf(s1.rdbuf());
    int n;
    s1 >> n;
    std::cout << ',' << n << '\n';
}
```

Output:

```text
10,20
```

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istream/%7Ebasic_istream*
