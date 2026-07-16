# std::destroy_at

```cpp
template< class T >
void destroy_at( T* p );  // (since C++17) (until C++20)
template< class T >
constexpr void destroy_at( T* p );  // (since C++20)
```

If `T` is not an array type, calls the destructor of the object pointed to by
`p`, as if by `p->~T()`.

If `T` is an array type, the program is ill-formed(until C++20)recursively
destroys elements of `*p` in order, as if by calling
`std::destroy(std::begin(*p), std::end(*p))`(since C++20).

### Parameters

- **p** — a pointer to the object to be destroyed

### Return value

(none)

### Possible implementation

```cpp
template<class T>
constexpr void destroy_at(T* p)
{
    if constexpr (std::is_array_v<T>)
        for (auto &elem : *p)
            (destroy_at)(std::addressof(elem));
    else
        p->~T();
}
// C++17 version:
// template<class T> void destroy_at(T* p) { p->~T(); }
```

### Notes

`destroy_at` deduces the type of object to be destroyed and hence avoids writing
it explicitly in the destructor call.

When `destroy_at` is called in the evaluation of some constant expression `e`,
the argument `p` must point to an object whose lifetime began within the
evaluation of `e`.
*(since C++20)*

### Example

The following example demonstrates how to use `destroy_at` to destroy a
contiguous sequence of elements.

```cpp
#include <iostream>
#include <memory>
#include <new>

struct Tracer
{
    int value;
    ~Tracer() { std::cout << value << " destructed\n"; }
};

int main()
{
    alignas(Tracer) unsigned char buffer[sizeof(Tracer) * 8];

    for (int i = 0; i < 8; ++i)
        new(buffer + sizeof(Tracer) * i) Tracer{i}; //manually construct objects

    auto ptr = std::launder(reinterpret_cast<Tracer*>(buffer));

    for (int i = 0; i < 8; ++i)
        std::destroy_at(ptr + i);
}
```

Output:

```text
0 destructed
1 destructed
2 destructed
3 destructed
4 destructed
5 destructed
6 destructed
7 destructed
```

### See also

- **destroy (C++17)** — destroys a range of objects (function template)
- **destroy_n (C++17)** — destroys a number of objects in a range (function
  template)
- **construct_at (C++20)** — creates an object at a given address (function
  template)
- **ranges::destroy_at (C++20)** — destroys an object at a given address
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/memory/destroy_at*
