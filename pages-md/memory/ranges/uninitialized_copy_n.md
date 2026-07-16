# std::ranges::uninitialized_copy_n, std::ranges::uninitialized_copy_n_result

```cpp
Call signature
template< std::input_iterator I, no-throw-input-iterator O, no-throw-sentinel-for<O> S >
requires std::constructible_from<std::iter_value_t<O>, std::iter_reference_t<I>>
         uninitialized_copy_n_result<I, O>
         uninitialized_copy_n( I ifirst, std::iter_difference_t<I> count,
                               O ofirst, S olast );  // (1) (since C++20)
Helper types
template< class I, class O >
using uninitialized_copy_n_result = ranges::in_out_result<I, O>;  // (2) (since C++20)
```

Let \(\scriptsize N\)N be `ranges::min(count, ranges::distance(ofirst, olast))`,
constructs \(\scriptsize N\)N elements in the output range
`[``ofirst``,``olast``)`, which is an uninitialized memory area, from the
elements in the input range beginning at `ifirst`.

The input range `[``ifirst``,``ifirst + count``)` must not overlap with the
output range `[``ofirst``,``olast``)`.

If an exception is thrown during the initialization, the objects already
constructed are destroyed in an unspecified order.

The function has the effect equivalent to:

```cpp
auto ret = ranges::uninitialized_copy(std::counted_iterator(ifirst, count),
                                      std::default_sentinel, ofirst, olast);
return {std::move(ret.in).base(), ret.out};
```

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **ifirst** — the beginning of the range of elements to copy from
- **count** — the number of elements to copy
- **ofirst, olast** — iterator-sentinel pair denoting the destination range

### Return value

`{ifirst + N, ofirst + N}`

### Complexity

\(\scriptsize\mathcal{O}(N)\)𝓞(N).

### Exceptions

The exception thrown on construction of the elements in the destination range,
if any.

### Notes

An implementation may improve the efficiency of the
`ranges::uninitialized_copy_n`, by using e.g. `ranges::copy_n`, if the value
type of the output range is TrivialType.

### Possible implementation

```cpp
struct uninitialized_copy_n_fn
{
    template<std::input_iterator I, no-throw-input-iterator O, no-throw-sentinel-for<O> S>
    requires std::constructible_from<std::iter_value_t<O>, std::iter_reference_t<I>>
    ranges::uninitialized_copy_n_result<I, O>
    operator()(I ifirst, std::iter_difference_t<I> count, O ofirst, S olast) const
    {
        O current{ofirst};
        try
        {
            for (; count > 0 && current != olast; ++ifirst, ++current, --count)
                ranges::construct_at(std::addressof(*current), *ifirst);
            return {std::move(ifirst), std::move(current)};
        }
        catch (...) // rollback: destroy constructed elements
        {
            for (; ofirst != current; ++ofirst)
                ranges::destroy_at(std::addressof(*ofirst));
            throw;
        }
    }
};

inline constexpr uninitialized_copy_n_fn uninitialized_copy_n{};
```

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <memory>
#include <string>

int main()
{
    const char* stars[]{"Procyon", "Spica", "Pollux", "Deneb", "Polaris"};

    constexpr int n{4};
    alignas(alignof(std::string)) char out[n * sizeof(std::string)];

    try
    {
        auto first{reinterpret_cast<std::string*>(out)};
        auto last{first + n};
        auto ret{std::ranges::uninitialized_copy_n(std::begin(stars), n, first, last)};

        std::cout << '{';
        for (auto it{first}; it != ret.out; ++it)
            std::cout << (it == first ? "" : ", ") << std::quoted(*it);
        std::cout << "};\n";

        std::ranges::destroy(first, last);
    }
    catch (...)
    {
        std::cout << "uninitialized_copy_n exception\n";
    }
}
```

Output:

```text
{"Procyon", "Spica", "Pollux", "Deneb"};
```

### See also

- **ranges::uninitialized_copy (C++20)** — copies a range of objects to an
  uninitialized area of memory (niebloid)
- **uninitialized_copy_n (C++11)** — copies a number of objects to an
  uninitialized area of memory (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/ranges/uninitialized_copy_n*
