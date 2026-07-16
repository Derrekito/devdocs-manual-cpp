# std::strstreambuf::~strstreambuf

```cpp
virtual ~strstreambuf();
```

Destroys a `std::strstreambuf` object. if the object is managing a
dynamically-allocated buffer (the buffer state is "allocated") and if the object
is not frozen, then deallocates the buffer using the deallocation function
provided at construction or `delete[]` if none was provided.

### Parameters

(none)

### Notes

This destructor is typically called by the destructor of `std::strstream`.

If `str()` was called on a dynamic strstream and `freeze(false)` was not called
after that, this destructor leaks memory.

### Example

```cpp
#include <iostream>
#include <strstream>

void* my_alloc(size_t n)
{
    std::cout << "my_alloc(" << n << ") called\n";
    return new char[n];
}

void my_free(void* p)
{
    std::cout << "my_free() called\n";
    delete[] (char*)p;
}

int main()
{
    {
        std::strstreambuf buf(my_alloc, my_free);
        std::ostream s(&buf);
        s << 1.23 << std::ends;
        std::cout << buf.str() << '\n';
        buf.freeze(false);
    } // destructor called here, buffer deallocated

    {
        std::strstreambuf buf(my_alloc, my_free);
        std::ostream s(&buf);
        s << 1.23 << std::ends;
        std::cout << buf.str() << '\n';
//      buf.freeze(false);
    } // destructor called here, memory leak!
}
```

Output:

```text
my_alloc(4096) called
1.23
my_free() called
my_alloc(4096) called
1.23
```

---
*Source: https://en.cppreference.com/w/cpp/io/strstreambuf/%7Estrstreambuf*
