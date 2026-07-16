# std::basic_ostream<CharT,Traits>::~basic_ostream

```cpp
virtual ~basic_ostream();
```

Destroys the `basic_ostream` object. This destructor does not perform any
operation on the underlying streambuffer (`rdbuf()`): the destructors of the
derived output streams such as `std::basic_ofstream` and
`std::basic_ostringstream` are responsible for calling the destructors of the
stream buffers.

### Example

```cpp
#include <iostream>
#include <sstream>

void add_words(std::streambuf* p)
{
    std::ostream buf(p); // buf shares the buffer with s
    buf << " is the answer";
} // calls the destructor of buf. p remains unaffected

int main()
{
    std::ostringstream s;
    s << 42;
    add_words(s.rdbuf());
    s << ".";
    std::cout << s.str() << '\n';
}
```

Output:

```text
42 is the answer.
```

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ostream/%7Ebasic_ostream*
