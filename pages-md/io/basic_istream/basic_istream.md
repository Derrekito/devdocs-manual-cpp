# std::basic_istream<CharT,Traits>::basic_istream

```cpp
explicit basic_istream( std::basic_streambuf<CharT, Traits>* sb );  // (1)
protected:
basic_istream( const basic_istream& rhs ) = delete;  // (2) (since C++11)
protected:
basic_istream( basic_istream&& rhs );  // (3) (since C++11)
```

1) Constructs the `basic_istream` object, assigning initial values to the base
class by calling `basic_ios::init(sb)`. The value of `gcount()` is initialized
to zero.

2) The copy constructor is protected, and is deleted. Input streams are not
copyable.

3) The move constructor copies the value of `gcount()` from `rhs`, sets the
gcount() value of `rhs` to zero, and uses `basic_ios<CharT, Traits>::move(rhs)`
to move all basic_ios members, except for the `rdbuf()`, from `rhs` into
`*this`. This move constructor is protected: it is called by the move
constructors of movable input stream classes `std::basic_ifstream` and
`std::basic_istringstream`, which know how to correctly move the associated
stream buffer.

### Parameters

- **sb** — streambuffer to use as underlying device

### Example

```cpp
#include <iostream>
#include <sstream>

int main()
{
    std::istringstream s1("hello");
    std::istream s2(s1.rdbuf()); // OK: s2 shares the buffer with s1

//  std::istream s3(std::istringstream("test")); // ERROR: move constructor is protected
//  std::istream s4(s2);                         // ERROR: copy constructor is deleted
    std::istringstream s5(std::istringstream("world")); // OK: move ctor called
                                                        //     by derived class

    std::cout << s2.rdbuf() << ' ' << s5.rdbuf() << '\n';
}
```

Output:

```text
hello world
```

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istream/basic_istream*
