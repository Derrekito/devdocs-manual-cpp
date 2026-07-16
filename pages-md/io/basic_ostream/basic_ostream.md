# std::basic_ostream<CharT,Traits>::basic_ostream

```cpp
explicit basic_ostream( std::basic_streambuf<CharT, Traits>* sb );  // (1)
protected:
basic_ostream( const basic_ostream& rhs ) = delete;  // (2) (since C++11)
protected:
basic_ostream( basic_ostream&& rhs );  // (3) (since C++11)
```

1) Constructs the `basic_ostream` object, assigning initial values to the base
   class by calling `basic_ios::init(sb)`.

2) The copy constructor is protected, and is deleted. Output streams are not
   copyable.

3) The move constructor uses `basic_ios<CharT, Traits>::move(rhs)` to move all
   `basic_ios` members, except for the `rdbuf()`, from `rhs` into `*this`. This
   move constructor is protected: it is called by the move constructors of
   movable output stream classes `std::basic_ofstream` and
   `std::basic_ostringstream`, which know how to correctly move the associated
   streambuffer.

### Parameters

- **sb** — streambuffer to use as output sequence
- **rhs** — basic_ostream to initialize from

### Notes

Because `basic_ios::init(sb)` sets `badbit` when `sb` is a null pointer, and
because `basic_ostream::sentry` does nothing if the stream is already in a
failed state, writing to a stream constructed from a null pointer `sb` is a
no-op.

### Example

```cpp
#include <iostream>
#include <sstream>
#include <utility>

int main()
{
    // ERROR: copy ctor is deleted
//  std::ostream myout(std::cout);

    // OK: shares buffer with cout
    std::ostream myout(std::cout.rdbuf());

    // ERROR: move constructor is protected
//  std::ostream s2(std::move(std::ostringstream() << 7.1));

    // OK: move ctor called through the derived class
    std::ostringstream s2(std::ostringstream() << 7.1);
    myout << s2.str() << '\n';

    std::ostream dev_null{nullptr}; // see Notes above
    dev_null << "no-op";
}
```

Output:

```text
7.1
```

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ostream/basic_ostream*
