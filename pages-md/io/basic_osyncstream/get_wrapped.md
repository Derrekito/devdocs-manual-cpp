# std::basic_osyncstream<CharT,Traits,Allocator>::get_wrapped

```cpp
streambuf_type* get_wrapped() const noexcept;
```

Returns a pointer to the wrapped `std::basic_streambuf`, obtained by calling
`get_wrapped()` on the underlying `std::basic_syncbuf`.

### Parameters

(none)

### Example

Wrapped buffer can be safely wrapped again in a different synchronized output
stream.

```cpp
#include <iostream>
#include <syncstream>

int main()
{
    std::osyncstream bout1(std::cout);
    bout1 << "Hello, ";
    {
        std::osyncstream(bout1.get_wrapped()) << "Goodbye, " << "Planet!" << '\n';
    } // emits the contents of the temporary buffer
    bout1 << "World!" << '\n';
} // emits the contents of bout1
```

Output:

```text
Goodbye, Planet!
Hello, World!
```

### See also

- **(destructor)** — destroys the `basic_osyncstream` and emits its internal
  buffer (public member function)
- **get_wrapped** — retrieves the wrapped streambuf pointer (public member
  function of `std::basic_syncbuf<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_osyncstream/get_wrapped*
