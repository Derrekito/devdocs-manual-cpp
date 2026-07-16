# std::basic_ios<CharT,Traits>::rdbuf

```cpp
std::basic_streambuf<CharT, Traits>* rdbuf() const;  // (1)
std::basic_streambuf<CharT, Traits>* rdbuf( std::basic_streambuf<CharT, Traits>* sb );  // (2)
```

Manages the associated stream buffer.

1) Returns the associated stream buffer. If there is no associated stream
   buffer, returns a null pointer.

2) Sets the associated stream buffer to `sb`. The error state is cleared by
   calling `clear()`. Returns the associated stream buffer before the operation.
   If there is no associated stream buffer, returns a null pointer.

### Parameters

- **sb** — stream buffer to associate to

### Return value

The associated stream buffer, or a null pointer if there was no associated
stream buffer.

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <iostream>
#include <sstream>

int main()
{
    std::ostringstream local;
    auto cout_buff = std::cout.rdbuf(); // save pointer to std::cout buffer

    std::cout.rdbuf(local.rdbuf()); // substitute internal std::cout buffer with
        // buffer of 'local' object

    // now std::cout work with 'local' buffer
    // you don't see this message
    std::cout << "some message";

    // go back to old buffer
    std::cout.rdbuf(cout_buff);

    // you will see this message
    std::cout << "back to default buffer\n";

    // print 'local' content
    std::cout << "local content: " << local.str() << "\n";
}
```

Output:

```text
back to default buffer
local content: some message
```

### See also

- **set_rdbuf** — replaces the `rdbuf` without clearing its error state
  (protected member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/rdbuf*
