# std::ranges::uninitialized_fill

```cpp
Call signature
template< no-throw-forward-iterator I, no-throw-sentinel-for<I> S, class T >
requires std::constructible_from<std::iter_value_t<I>, const T&>
I uninitialized_fill( I first, S last, const T& x );  // (1) (since C++20)
template< no-throw-forward-range R, class T >
requires std::constructible_from<ranges::range_value_t<R>, const T&>
ranges::borrowed_iterator_t<R>
uninitialized_fill( R&& r, const T& x );  // (2) (since C++20)
```

1) Constructs \(\scriptsize N\)N copies of the given value `x` in an
   uninitialized memory area, designated by the range `[``first``,``last``)`,
   where \(\scriptsize N\)N is `ranges::distance(first, last)`.

The function has the effect equivalent to: for (; first != last; ++first) ::new
   (static_cast<void*>(std::addressof(*first)))
   std::remove_reference_t<std::iter_reference_t<I>>(x); return first;

If an exception is thrown during the initialization, the objects already
   constructed are destroyed in an unspecified order.

2) Same as (1), but uses `r` as the range, as if using `ranges::begin(r)` as
   `first`, and `ranges::end(r)` as `last`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — iterator-sentinel pair denoting the range of elements to
  initialize
- **r** — the range of the elements to initialize
- **value** — the value to construct the elements with

### Return value

An iterator equal to `last`.

### Complexity

\(\scriptsize\mathcal{O}(N)\)𝓞(N).

### Exceptions

The exception thrown on construction of the elements in the destination range,
if any.

### Notes

An implementation may improve the efficiency of the
`ranges::uninitialized_fill`, e.g. by using `ranges::fill`, if the value type of
the output range is TrivialType.

### Possible implementation

```cpp
struct uninitialized_fill_fn
{
    template<no-throw-forward-iterator I, no-throw-sentinel-for<I> S, class T>
    requires std::constructible_from<std::iter_value_t<I>, const T&>
    I operator()(I first, S last, const T& x) const
    {
        I rollback{first};
        try
        {
            for (; !(first == last); ++first)
                ranges::construct_at(std::addressof(*first), x);
            return first;
        }
        catch (...)
        {
            // rollback: destroy constructed elements
            for (; rollback != first; ++rollback)
                ranges::destroy_at(std::addressof(*rollback));
            throw;
        }
    }

    template<no-throw-forward-range R, class T>
    requires std::constructible_from<ranges::range_value_t<R>, const T&>
    ranges::borrowed_iterator_t<R>
    operator()(R&& r, const T& x) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), x);
    }
};

inline constexpr uninitialized_fill_fn uninitialized_fill{};
```

### Example

```cpp
#include <iostream>
#include <memory>
#include <string>

int main()
{
    constexpr int n{4};
    alignas(alignof(std::string)) char out[n * sizeof(std::string)];

    try
    {
        auto first{reinterpret_cast<std::string*>(out)};
        auto last{first + n};
        std::ranges::uninitialized_fill(first, last, "▄▀▄▀▄▀▄▀");

        int count{1};
        for (auto it{first}; it != last; ++it)
            std::cout << count++ << ' ' << *it << '\n';

        std::ranges::destroy(first, last);
    }
    catch(...)
    {
        std::cout << "Exception!\n";
    }
}
```

Output:

```text
1 ▄▀▄▀▄▀▄▀
2 ▄▀▄▀▄▀▄▀
3 ▄▀▄▀▄▀▄▀
4 ▄▀▄▀▄▀▄▀
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3870 | C++20 | this algorithm might create objects on a const storage |
      kept disallowed

### See also

- **ranges::uninitialized_fill_n (C++20)** — copies an object to an
  uninitialized area of memory, defined by a start and a count (niebloid)
- **uninitialized_fill** — copies an object to an uninitialized area of memory,
  defined by a range (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/ranges/uninitialized_fill*
