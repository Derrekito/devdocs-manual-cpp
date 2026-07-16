# std::ranges::destroy_at

```cpp
Call signature
template< std::destructible T >
constexpr void destroy_at( T* p ) noexcept;  // (since C++20)
```

If `T` is not an array type, calls the destructor of the object pointed to by
`p`, as if by `p->~T()`. Otherwise, recursively destroys elements of `*p` in
order, as if by calling `std::destroy(std::begin(*p), std::end(*p))`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **p** — a pointer to the object to be destroyed

### Return value

(none)

### Possible implementation

```cpp
struct destroy_at_fn
{
    template<std::destructible T>
    constexpr void operator()(T *p) const noexcept
    {
        if constexpr (std::is_array_v<T>)
            for (auto &elem : *p)
                operator()(std::addressof(elem));
        else
            p->~T();
    }
};

inline constexpr destroy_at_fn destroy_at{};
```

### Notes

`destroy_at` deduces the type of object to be destroyed and hence avoids writing
it explicitly in the destructor call.

When `destroy_at` is called in the evaluation of some constant expression `e`,
the argument `p` must point to an object whose lifetime began within the
evaluation of `e`.

### Example

The following example demonstrates how to use `ranges::destroy_at` to destroy a
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
        std::ranges::destroy_at(ptr + i);
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

- **ranges::destroy (C++20)** — destroys a range of objects (niebloid)
- **ranges::destroy_n (C++20)** — destroys a number of objects in a range
  (niebloid)
- **ranges::construct_at (C++20)** — creates an object at a given address
  (niebloid)
- **destroy_at (C++17)** — destroys an object at a given address (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/memory/ranges/destroy_at*
