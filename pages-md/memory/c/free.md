# std::free

```cpp
void free( void* ptr );
```

Deallocates the space previously allocated by `std::malloc`, `std::calloc`,
`std::aligned_alloc`(since C++17), or `std::realloc`.

If `ptr` is a null pointer, the function does nothing.

The behavior is undefined if the value of `ptr` does not equal a value returned
earlier by `std::malloc`, `std::calloc`, `std::aligned_alloc`(since C++17), or
`std::realloc`.

The behavior is undefined if the memory area referred to by `ptr` has already
been deallocated, that is, `std::free` or `std::realloc` has already been called
with `ptr` as the argument and no calls to `std::malloc`, `std::calloc`,
`std::aligned_alloc`(since C++17), or `std::realloc` resulted in a pointer equal
to `ptr` afterwards.

The behavior is undefined if after `std::free` returns, an access is made
through the pointer `ptr` (unless another allocation function happened to result
in a pointer value equal to `ptr`).

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

- **ptr** — pointer to the memory to deallocate

### Return value

(none)

### Notes

The function accepts (and does nothing with) the null pointer to reduce the
amount of special-casing. Whether allocation succeeds or not, the pointer
returned by an allocation function can be passed to `std::free`.

### Example

```cpp
#include <cstdlib>

int main()
{
    int* p1 = (int*)std::malloc(10 * sizeof *p1);
    std::free(p1); // every allocated pointer must be freed

    int* p2 = (int*)std::calloc(10, sizeof *p2);
    int* p3 = (int*)std::realloc(p2, 1000 * sizeof *p3);
    if (!p3) // p3 null means realloc failed and p2 must be freed.
        std::free(p2);
    std::free(p3); // p3 can be freed whether or not it is null.
}
```

### See also

**C documentation for `free`**

---
*Source: https://en.cppreference.com/w/cpp/memory/c/free*
