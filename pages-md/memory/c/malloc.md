# std::malloc

```cpp
void* malloc( std::size_t size );
```

Allocates `size` bytes of uninitialized storage.

If allocation succeeds, returns a pointer to the lowest (first) byte in the
allocated memory block that is suitably aligned for any scalar type (at least as
strictly as `std::max_align_t`) (implicitly creating objects in the destination
area).

If `size` is zero, the behavior is implementation defined (null pointer may be
returned, or some non-null pointer may be returned that may not be used to
access storage, but has to be passed to `std::free`).

The following functions are required to be thread-safe:
- The library versions of `operator new` and `operator delete`
- User replacement versions of global `operator new` and `operator delete`
- `std::calloc`, `std::malloc`, `std::realloc`, `std::aligned_alloc`(since
  C++17), `std::free`
Calls to these functions that allocate or deallocate a particular unit of
storage occur in a single total order, and each such deallocation call
happens-before the next allocation (if any) in this order.
*(since C++11)*

### Parameters

- **size** — number of bytes to allocate

### Return value

On success, returns the pointer to the beginning of newly allocated memory. To
avoid a memory leak, the returned pointer must be deallocated with `std::free()`
or `std::realloc()`.

On failure, returns a null pointer.

### Notes

This function does not call constructors or initialize memory in any way. There
are no ready-to-use smart pointers that could guarantee that the matching
deallocation function is called. The preferred method of memory allocation in
C++ is using RAII-ready functions `std::make_unique`, `std::make_shared`,
container constructors, etc, and, in low-level library code, new-expression.

For loading a large file, file mapping via OS-specific functions, e.g. `mmap` on
POSIX or `CreateFileMapping`(`A`/`W`) along with `MapViewOfFile` on Windows, is
preferable to allocating a buffer for file reading.

### Example

```cpp
#include <cstdlib>
#include <iostream>
#include <memory>
#include <string>

int main()
{
    constexpr std::size_t size = 4;
    if (auto ptr = reinterpret_cast<std::string*>(std::malloc(size * sizeof(std::string))))
    {
        try
        {
            for (std::size_t i = 0; i < size; ++i)
                std::construct_at(ptr + i, 5, 'a' + i);
            for (std::size_t i = 0; i < size; ++i)
                std::cout << "ptr[" << i << "] == " << ptr[i] << '\n';
            std::destroy_n(ptr, size);
        }
        catch (...) {}
        std::free(ptr);
    }
}
```

Output:

```text
p[0] == aaaaa
p[1] == bbbbb
p[2] == ccccc
p[3] == ddddd
```

### See also

- **operator newoperator new[]** — allocation functions (function)
- **get_temporary_buffer (deprecated in C++17)(removed in C++20)** — obtains
  uninitialized storage (function template)

**C documentation for `malloc`**

---
*Source: https://en.cppreference.com/w/cpp/memory/c/malloc*
