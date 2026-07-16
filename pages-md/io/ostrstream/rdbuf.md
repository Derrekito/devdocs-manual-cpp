# std::ostrstream::rdbuf

```cpp
strstreambuf* rdbuf() const;
```

Returns a pointer to the associated `std::strstreambuf`, casting away its
constness (despite the const qualifier on the member function).

### Parameters

(none)

### Return value

Pointer to the associated `std::strsteambuf`, with constness cast away.

### Example

```cpp
#include <strstream>

int main()
{
    const std::ostrstream buf;
    std::strstreambuf* ptr = buf.rdbuf();
}
```

---
*Source: https://en.cppreference.com/w/cpp/io/ostrstream/rdbuf*
