# std::basic_osyncstream<CharT,Traits,Allocator>::emit

```cpp
void emit();
```

Emits all buffered output and executes any pending flushes, by calling `emit()`
on the underlying `std::basic_syncbuf`.

### Parameters

(none)

### Example

```cpp
#include <iostream>
#include <syncstream>

int main()
{
    {
        std::osyncstream bout(std::cout);
        bout << "Hello," << '\n'; // no flush
        bout.emit(); // characters transferred; cout not flushed
        bout << "World!" << std::endl; // flush noted; cout not flushed
        bout.emit(); // characters transferred; cout flushed
        bout << "Greetings." << '\n'; // no flush
    } // destructor calls emit(): characters transferred; cout not flushed

    // emit can be used for local exception-handling on the wrapped stream
    std::osyncstream bout(std::cout);
    bout << "Hello, " << "World!" << '\n';
    try
    {
        bout.emit();
    }
    catch (...)
    {
        // handle exceptions
    }
}
```

Output:

```text
Hello,
World!
Greetings.
Hello, World!
```

### See also

- **(destructor)** — destroys the `basic_osyncstream` and emits its internal
  buffer (public member function)
- **emit** — atomically transmits the entire internal buffer to the wrapped
  streambuf (public member function of
  `std::basic_syncbuf<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_osyncstream/emit*
