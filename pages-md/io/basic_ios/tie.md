# std::basic_ios<CharT,Traits>::tie

```cpp
std::basic_ostream<CharT, Traits>* tie() const;  // (1)
std::basic_ostream<CharT, Traits>* tie( std::basic_ostream<CharT, Traits>* str );  // (2)
```

Manages the tied stream. A tied stream is an output stream which is synchronized
with the sequence controlled by the stream buffer (`rdbuf()`), that is,
`flush()` is called on the tied stream before any input/output operation on
`*this`.

1) Returns the current tied stream. If there is no tied stream, a null pointer
   is returned.

2) Sets the current tied stream to `str`. Returns the tied stream before the
   operation. If there is no tied stream, a null pointer is returned. If `str`
   is not null and `tie()` is reachable by traversing the linked list of tied
   stream objects starting from `str->tie()`, the behavior is undefined.

### Parameters

- **str** — an output stream to set as the tied stream

### Return value

The tied stream, or a null pointer if there was no tied stream.

### Exceptions

May throw implementation-defined exceptions.

### Notes

By default, the standard stream `std::cout` is tied to `std::cin` and
`std::cerr`. Similarly, its wide counterpart `std::wcout` is tied to `std::wcin`
and `std::wcerr`.

### Example

```cpp
#include <fstream>
#include <iomanip>
#include <iostream>
#include <string>

int main()
{
    std::ofstream os("test.txt");
    std::ifstream is("test.txt");
    std::string value("0");

    os << "Hello";
    is >> value;

    std::cout << "Result before tie(): " << std::quoted(value) << "\n";
    is.clear();
    is.tie(&os);

    is >> value;

    std::cout << "Result after tie(): " << std::quoted(value) << "\n";
}
```

Output:

```text
Result before tie(): "0"
Result after tie(): "Hello"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 835 | C++98 | two streams could be tied to each other[1] (either directly
      or through another intermediate stream object) | the behavior is undefined
      in this case

1. `std::basic_ostream::flush()` is an UnformattedOutputFunction, so it creates
   a sentry object while being called. When `flush()` is called on a stream
   object, the constructor of the sentry object will call `flush()` on its tied
   stream, and that `flush()` will construct another sentry object and its
   constructor will call `flush()` on the tied stream of that stream and so on.
   Therefore, if streams `a` and `b` are (directly or indirectly) tied to each
   other, calling `a.flush()` will eventually call `b.flush()`, which will
   eventually call `a.flush()`, and will result in an infinite loop.

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/tie*
