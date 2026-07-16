# std::ostrstream::~ostrstream

```cpp
virtual ~ostrstream();
```

Destroys a `std::ostrstream` object, which also destroys the member
`std::strstreambuf`, which may call the deallocation function if the underlying
buffer was dynamically-allocated and not frozen.

### Parameters

(none)

### Notes

If `str()` was called on a dynamic ostrstream and freeze(false) was not called
after that, this destructor leaks memory.

### Example

```cpp
#include <iostream>
#include <strstream>

int main()
{
    {
        std::ostrstream s; // dynamic buffer
        s << 1.23;
        std::cout << s.str() << '\n';
        s.freeze(false);
    } // destructor called, buffer deallocated

    {
        std::ostrstream s;
        s << 1.23;
        std::cout << s.str() << '\n';
//      buf.freeze(false);
    } // destructor called, memory leaked
}
```

Output:

```text
1.23
1.23
```

---
*Source: https://en.cppreference.com/w/cpp/io/ostrstream/%7Eostrstream*
