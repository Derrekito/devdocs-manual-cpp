# std::ranges::uninitialized_copy, std::ranges::uninitialized_copy_result

```cpp
Call signature
template< std::input_iterator I, std::sentinel_for<I> S1,
          no-throw-forward-iterator O, no-throw-sentinel-for<O> S2 >
requires std::constructible_from<std::iter_value_t<O>, std::iter_reference_t<I>>
         uninitialized_copy_result<I, O>
         uninitialized_copy( I ifirst, S1 ilast, O ofirst, S2 olast );  // (1) (since C++20)
template< ranges::input_range IR, no-throw-forward-range OR >
requires std::constructible_from<ranges::range_value_t<OR>,
         ranges::range_reference_t<IR>>
         uninitialized_copy_result<ranges::borrowed_iterator_t<IR>,
                                   ranges::borrowed_iterator_t<OR>>
         uninitialized_copy( IR&& in_range, OR&& out_range );  // (2) (since C++20)
Helper types
template< class I, class O >
using uninitialized_copy_result = ranges::in_out_result<I, O>;  // (3) (since C++20)
```

1) Let \(\scriptsize N\)N be `ranges::min(ranges::distance(ifirst, ilast),
   ranges::distance(ofirst, olast))`, constructs \(\scriptsize N\)N elements in
   the output range `[``ofirst``,``olast``)`, which is an uninitialized memory
   area, from the elements in the input range `[``ifirst``,``ilast``)`.

The input and output ranges must not overlap.

If an exception is thrown during the initialization, the objects already
   constructed are destroyed in an unspecified order.

The function has the effect equal to: for (; !(ifirst == ilast || ofirst ==
   olast); ++ofirst, ++ifirst) { ::new
   (static_cast<void*>(std::addressof(*ofirst)))
   std::remove_reference_t<std::iter_reference_t<O>>(*ifirst); }

2) Same as (1), but uses `in_range` as the first range and `out_range` as the
   second range, as if using `ranges::begin(in_range)` as `ifirst`,
   `ranges::end(in_range)` as `ilast`, `ranges::begin(out_range)` as `ofirst`,
   and `ranges::end(out_range)` as `olast`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **ifirst, ilast** — iterator-sentinel pair denoting the range of elements to
  copy from
- **in_range** — the range of elements to copy from
- **ofirst, olast** — iterator-sentinel pair denoting the destination range
- **out_range** — the destination range

### Return value

`{ifirst + N, ofirst + N}`

### Complexity

\(\scriptsize\mathcal{O}(N)\)𝓞(N).

### Exceptions

The exception thrown on construction of the elements in the destination range,
if any.

### Notes

An implementation may improve the efficiency of `ranges::uninitialized_copy` if
the value type of the output range is TrivialType.

### Possible implementation

```cpp
struct uninitialized_copy_fn
{
    template<std::input_iterator I, std::sentinel_for<I> S1,
             no-throw-forward-iterator O, no-throw-sentinel-for<O> S2>
    requires std::constructible_from<std::iter_value_t<O>, std::iter_reference_t<I>>
    ranges::uninitialized_copy_result<I, O>
    operator()(I ifirst, S1 ilast, O ofirst, S2 olast) const
    {
        O current{ofirst};
        try
        {
            for (; !(ifirst == ilast or current == olast); ++ifirst, ++current)
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

    template<ranges::input_range IR, no-throw-forward-range OR>
    requires std::constructible_from<ranges::range_value_t<OR>,
             ranges::range_reference_t<IR>>
    ranges::uninitialized_copy_result<ranges::borrowed_iterator_t<IR>,
                                      ranges::borrowed_iterator_t<OR>>
    operator()(IR&& in_range, OR&& out_range) const
    {
        return (*this)(ranges::begin(in_range), ranges::end(in_range),
                       ranges::begin(out_range), ranges::end(out_range));
    }
};

inline constexpr uninitialized_copy_fn uninitialized_copy{};
```

### Example

```cpp
#include <cstdlib>
#include <iomanip>
#include <iostream>
#include <memory>
#include <string>

int main()
{
    const char* v[]{"This", "is", "an", "example"};

    if (const auto sz{std::size(v)};
        void* pbuf = std::aligned_alloc(alignof(std::string), sizeof(std::string) * sz))
    {
        try
        {
            auto first{static_cast<std::string*>(pbuf)};
            auto last{first + sz};
            std::ranges::uninitialized_copy(std::begin(v), std::end(v), first, last);

            std::cout << "{";
            for (auto it{first}; it != last; ++it)
                std::cout << (it == first ? "" : ", ") << std::quoted(*it);
            std::cout << "};\n";

            std::ranges::destroy(first, last);
        }
        catch (...)
        {
            std::cout << "uninitialized_copy exception\n";
        }
        std::free(pbuf);
    }
}
```

Output:

```text
{"This", "is", "an", "example"};
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3870 | C++20 | this algorithm might create objects on a const storage |
      kept disallowed

### See also

- **ranges::uninitialized_copy_n (C++20)** — copies a number of objects to an
  uninitialized area of memory (niebloid)
- **uninitialized_copy** — copies a range of objects to an uninitialized area of
  memory (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/ranges/uninitialized_copy*
