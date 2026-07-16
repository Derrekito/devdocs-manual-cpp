# std::ranges::uninitialized_fill_n

```cpp
Call signature
template< no-throw-forward-range I, class T >
requires std::constructible_from<std::iter_value_t<I>, const T&>
I uninitialized_fill_n( I first, std::iter_difference_t<I> n, const T& x );  // (since C++20)
```

Constructs `n` copies of the given value `x` in an uninitialized memory area,
designated by the range `[``first``,``first + n``)`, as if by

```cpp
for (; n--; ++first) {
    ::new (static_cast<void*>(std::addressof(*first)))
        std::remove_reference_t<std::iter_reference_t<I>>(x);
}
```

If an exception is thrown during the initialization, the objects already
constructed are destroyed in an unspecified order.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first** — the beginning of the range of the elements to initialize
- **n** — number of elements to construct
- **x** — the value to construct the elements with

### Return value

An iterator equal to `first + n`.

### Complexity

Linear in `n`.

### Exceptions

The exception thrown on construction of the elements in the destination range,
if any.

### Notes

An implementation may improve the efficiency of the
`ranges::uninitialized_fill_n`, e.g. by using `ranges::fill_n`, if the value
type of the output range is TrivialType.

### Possible implementation

```cpp
struct uninitialized_fill_n_fn
{
    template<no-throw-forward-range I, class T>
    requires std::constructible_from<std::iter_value_t<I>, const T&>
    I operator()(I first, std::iter_difference_t<I> n, const T& x) const
    {
        I rollback{first};
        try
        {
            for (; n-- > 0; ++first)
                ranges::construct_at(std::addressof(*first), x);
            return first;
        }
        catch (...) // rollback: destroy constructed elements
        {
            for (; rollback != first; ++rollback)
                ranges::destroy_at(std::addressof(*rollback));
            throw;
        }
    }
};

inline constexpr uninitialized_fill_n_fn uninitialized_fill_n{};
```

### Example

```cpp
#include <iostream>
#include <memory>
#include <string>

int main()
{
    constexpr int n{3};
    alignas(alignof(std::string)) char out[n * sizeof(std::string)];

    try
    {
        auto first{reinterpret_cast<std::string*>(out)};
        auto last = std::ranges::uninitialized_fill_n(first, n, "cppreference");

        for (auto it{first}; it != last; ++it)
            std::cout << *it << '\n';

        std::ranges::destroy(first, last);
    }
    catch (...)
    {
        std::cout << "Exception!\n";
    }
}
```

Output:

```text
cppreference
cppreference
cppreference
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3870 | C++20 | this algorithm might create objects on a const storage |
      kept disallowed

### See also

- **ranges::uninitialized_fill (C++20)** — copies an object to an uninitialized
  area of memory, defined by a range (niebloid)
- **uninitialized_fill_n** — copies an object to an uninitialized area of
  memory, defined by a start and a count (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/ranges/uninitialized_fill_n*
