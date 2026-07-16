# std::ranges::destroy

```cpp
Call signature
template< no-throw-input-iterator I, no-throw-sentinel-for<I> S >
    requires std::destructible<std::iter_value_t<I>>
constexpr I destroy( I first, S last ) noexcept;  // (1) (since C++20)
template< no-throw-input-range R >
    requires std::destructible<ranges::range_value_t<R>>
constexpr ranges::borrowed_iterator_t<R> destroy( R&& r ) noexcept;  // (2) (since C++20)
```

1) Destroys the objects in the range `[``first``,``last``)`, as if by for (;
   first != last; ++first) std::ranges::destroy_at(std::addressof(*first));
   return first;

2) Same as (1), but uses `r` as the source range, as if using `ranges::begin(r)`
   as `first` and `ranges::end(r)` as `last`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — iterator-sentinel pair denoting the range of elements to
  destroy
- **r** — the range to destroy

### Return value

An iterator compares equal to `last`.

### Complexity

Linear in the distance between `first` and `last`.

### Possible implementation

```cpp
struct destroy_fn
{
    template<no-throw-input-iterator I, no-throw-sentinel-for<I> S>
        requires std::destructible<std::iter_value_t<I>>
    constexpr I operator()(I first, S last) const noexcept
    {
        for (; first != last; ++first)
            std::ranges::destroy_at(std::addressof(*first));
        return first;
    }

    template<no-throw-input-range R>
        requires std::destructible<std::ranges::range_value_t<R>>
    constexpr std::ranges::borrowed_iterator_t<R> operator()(R&& r) const noexcept
    {
        return operator()(std::ranges::begin(r), std::ranges::end(r));
    }
};

inline constexpr destroy_fn destroy{};
```

### Example

The following example demonstrates how to use `ranges::destroy` to destroy a
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

    std::ranges::destroy(ptr, ptr + 8);
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

- **ranges::destroy_n (C++20)** — destroys a number of objects in a range
  (niebloid)
- **ranges::destroy_at (C++20)** — destroys an object at a given address
  (niebloid)
- **destroy (C++17)** — destroys a range of objects (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/ranges/destroy*
