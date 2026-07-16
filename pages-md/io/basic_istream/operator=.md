# std::basic_istream<CharT,Traits>::operator=

```cpp
protected:
basic_istream& operator=( const basic_istream& rhs ) = delete;  // (1)
protected:
basic_istream& operator=( basic_istream&& rhs );  // (2) (since C++11)
```

1) The copy assignment operator is protected, and is deleted. Input streams are
   not CopyAssignable.

2) The move assignment operator exchanges the `gcount()` values and all data
   members of the base class, except for `rdbuf()`, with `rhs`, as if by calling
   `swap(*rhs)`. This move assignment operator is protected: it is only called
   by the move assignment operators of the derived movable input stream classes
   `std::basic_ifstream` and `std::basic_istringstream`, which know how to
   correctly move-assign the associated streambuffers.

### Parameters

- **rhs** — the basic_istream object from which to assign to `*this`

### Example

```cpp
#include <iostream>
#include <sstream>

int main()
{
    std::istringstream s1;
    s1 = std::istringstream("test"); // OK

//  std::cin = std::istringstream("test"); // ERROR: 'operator=' is protected
}
```

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istream/operator%3D*
