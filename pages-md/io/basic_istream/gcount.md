# std::basic_istream<CharT,Traits>::gcount

```cpp
std::streamsize gcount() const;
```

Returns the number of characters extracted by the last unformatted input
operation, or the maximum representable value of `std::streamsize` if the number
is not representable.

The following member functions of `basic_istream` change the value of subsequent
`gcount()` calls:

- move constructor
- `swap()`
- `get()`
- `getline()`
- `ignore()`
- `read()`
- `readsome()`
- `operator>>(basic_streambuf*)`

The following functions set `gcount()` to zero:

- constructor
- `putback()`
- `unget()`
- `peek()`

### Parameters

(none)

### Return value

The number of characters extracted by the last unformatted input operation, or
the maximum representable value of `std::streamsize` if the number is not
representable.

### Example

```cpp
#include <iostream>
#include <sstream>

int main()
{
    char x[20];
    std::istringstream stream("Hello World");

    stream.read(x, sizeof x);
    std::cout << "Characters extracted: " << stream.gcount();
}
```

Output:

```text
Characters extracted: 11
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3464 | C++98 | the return value was unspecified when the result overflows
      | returns the maximum value

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istream/gcount*
