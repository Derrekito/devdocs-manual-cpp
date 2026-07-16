# std::ranges::destroy_n

```cpp
Call signature
template< no-throw-input-iterator I >
    requires std::destructible<std::iter_value_t<I>>
constexpr I destroy_n( I first, std::iter_difference_t<I> n ) noexcept;  // (since C++20)
```

Destroys the `n` objects in the range starting at `first`, equivalent to

```cpp
return std::ranges::destroy(std::counted_iterator(first, n), std::default_sentinel).base();
```

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first** — the beginning of the range of elements to destroy
- **n** — the number of elements to destroy

### Return value

The end of the range of objects that has been destroyed.

### Complexity

Linear in `n`.

### Possible implementation

```cpp
struct destroy_n_fn
{
    template<no-throw-input-iterator I>
        requires std::destructible<std::iter_value_t<I>>
    constexpr I operator()(I first, std::iter_difference_t<I> n) const noexcept
    {
        for (; n != 0; (void)++first, --n)
            std::ranges::destroy_at(std::addressof(*first));
        return first;
    }
};

inline constexpr destroy_n_fn destroy_n{};
```

### Example

The following example demonstrates how to use `ranges::destroy_n` to destroy a
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

    std::ranges::destroy_n(ptr, 8);
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

- **ranges::destroy_at (C++20)** — destroys an object at a given address
  (niebloid)
- **ranges::destroy (C++20)** — destroys a range of objects (niebloid)
- **destroy_n (C++17)** — destroys a number of objects in a range (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/memory/ranges/destroy_n*
