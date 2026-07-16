# std::align

```cpp
void* align( std::size_t alignment,
             std::size_t size,
             void*& ptr,
             std::size_t& space );  // (since C++11)
```

Given a pointer `ptr` to a buffer of size `space`, returns a pointer aligned by
the specified `alignment` for `size` number of bytes and decreases `space`
argument by the number of bytes used for alignment. The first aligned address is
returned.

The function modifies the pointer only if it would be possible to fit the wanted
number of bytes aligned by the given alignment into the buffer. If the buffer is
too small, the function does nothing and returns `nullptr`.

The behavior is undefined if `alignment` is not a power of two.

### Parameters

- **alignment** — the desired alignment
- **size** — the size of the storage to be aligned
- **ptr** — pointer to contiguous storage (a buffer) of at least `space` bytes
- **space** — the size of the buffer in which to operate

### Return value

The adjusted value of `ptr`, or null pointer value if the space provided is too
small.

### Example

Demonstrates the use of `std::align` to place objects of different type in
memory.

```cpp
#include <iostream>
#include <memory>

template<std::size_t N>
struct MyAllocator
{
    char data[N];
    void* p;
    std::size_t sz;
    MyAllocator() : p(data), sz(N) {}
    template<typename T>
    T* aligned_alloc(std::size_t a = alignof(T))
    {
        if (std::align(a, sizeof(T), p, sz))
        {
            T* result = reinterpret_cast<T*>(p);
            p = (char*)p + sizeof(T);
            sz -= sizeof(T);
            return result;
        }
        return nullptr;
    }
};

int main()
{
    MyAllocator<64> a;
    std::cout << "allocated a.data at " << (void*)a.data
              << " (" << sizeof a.data << " bytes)\n";

    // allocate a char
    if (char* p = a.aligned_alloc<char>())
    {
        *p = 'a';
        std::cout << "allocated a char at " << (void*)p << '\n';
    }

    // allocate an int
    if (int* p = a.aligned_alloc<int>())
    {
        *p = 1;
        std::cout << "allocated an int at " << (void*)p << '\n';
    }

    // allocate an int, aligned at 32-byte boundary
    if (int* p = a.aligned_alloc<int>(32))
    {
        *p = 2;
        std::cout << "allocated an int at " << (void*)p << " (32 byte alignment)\n";
    }
}
```

Possible output:

```text
allocated a.data at 0x7ffd0b331f80 (64 bytes)
allocated a char at 0x7ffd0b331f80
allocated an int at 0x7ffd0b331f84
allocated an int at 0x7ffd0b331fa0 (32 byte alignment)
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2377 | C++11 | `alignment` required to be a fundamental or supported
      extended alignment value | only need to be a power of two

### See also

- **`alignof` operator(C++11)** — queries alignment requirements of a type
- **`alignas` specifier(C++11)** — specifies that the storage for the variable
  should be aligned by specific amount
- **aligned_storage (C++11)(deprecated in C++23)** — defines the type suitable
  for use as uninitialized storage for types of given size (class template)
- **assume_aligned (C++20)** — informs the compiler that a pointer is aligned
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/align*
