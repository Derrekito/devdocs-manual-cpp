# std::basic_osyncstream<CharT,Traits,Allocator>::operator=

```cpp
basic_osyncstream& operator=( std::basic_osyncstream&& other );  // (since C++20)
```

Move-assigns a synchronized output stream:

Move-assigns the wrapped `std::basic_syncbuf` from the corresponding member of
`other` (after this move-assignment, `other.get_wrapped()` returns a null
pointer and destruction of `other` produces no output; any pending buffered
output will be emitted) and move-assigns the base `std::basic_ostream` (this
swaps all stream state variables except for `rdbuf` between `*this` and `other`)

### Parameters

- **other** — another synchronized output stream to move from

### Return value

`*this`

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <sstream>
#include <syncstream>
#include <utility>

int main()
{
    std::osyncstream out(std::cout);
    out << "test\n";
    std::ostringstream str_out;
    std::osyncstream{str_out} = std::move(out); // Note that out is emitted here
    std::cout << "str_out = " << std::quoted(str_out.view()) << '\n';
}
```

Output:

```text
test
str_out = ""
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3867 | C++20 | the move assignment operator was noexcept, but
      `std::basic_syncbuf`'s move assignment operator is not | removed noexcept

### See also

- **(constructor)** — constructs a `basic_osyncstream` object (public member
  function)
- **(destructor)** — destroys the `basic_osyncstream` and emits its internal
  buffer (public member function)
- **emit** — calls `emit()` on the underlying `basic_syncbuf` to transmit its
  internal data to the final destination (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_osyncstream/operator%3D*
