# std::ranges::clamp

```cpp
Call signature
template< class T, class Proj = std::identity,
          std::indirect_strict_weak_order<std::projected<const T*, Proj>> Comp =
              ranges::less >
constexpr const T&
    clamp( const T& v, const T& lo, const T& hi, Comp comp = {}, Proj proj = {} );  // (since C++20)
```

If `v` compares less than `lo`, returns `lo`; otherwise if `hi` compares less
than `v`, returns `hi`; otherwise returns `v`.

The behavior is undefined if `lo` is greater than `hi`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **v** — the value to clamp
- **lo, hi** — the boundaries to clamp `v` to
- **comp** — the comparison to apply to the projected elements
- **proj** — the projection to apply to `v`, `lo` and `hi`

### Return value

Reference to `lo` if the projected value of `v` is less than the projected value
of `lo`, reference to `hi` if the projected value of `hi` is less than the
projected value of `v`, otherwise reference to `v`.

### Complexity

At most two comparisons and three applications of the projection.

### Possible implementation

```cpp
struct clamp_fn
{
    template<class T, class Proj = std::identity,
             std::indirect_strict_weak_order<std::projected<const T*, Proj>>
                 Comp = std::ranges::less>
    constexpr const T& operator()(const T& v, const T& lo, const T& hi,
                                  Comp comp = {}, Proj proj = {}) const
    {
        auto&& pv = std::invoke(proj, v);

        if (std::invoke(comp, std::forward<decltype(pv)>(pv), std::invoke(proj, lo)))
            return lo;

        if (std::invoke(comp, std::invoke(proj, hi), std::forward<decltype(pv)>(pv)))
            return hi;

        return v;
    }
};

inline constexpr clamp_fn clamp;
```

### Notes

Capturing the result of `std::ranges::clamp` by reference produces a dangling
reference if one of the parameters is a temporary and that parameter is
returned:

```cpp
int n = -1;
const int& r = std::ranges::clamp(n, 0, 255); // r is dangling
```

If `v` compares equivalent to either bound, returns a reference to `v`, not the
bound.

This function should not be used with both a projection the returns by value and
comparator that takes arguments by value unless a move from the projection
result type to the comparator parameter type is equivalent to a copy. If the
comparison via `std::invoke` would change the result of projection, the behavior
is undefined due to the semantic requirements of `std::regular_invocable`
(subsumed by `std::indirect_strict_weak_order`).

The standard requires that the value category of the result of the projection be
preserved, and `proj` can only be called on `v` once, which means that a
projection result that is a prvalue has to be cached and moved from twice for
the two calls to the comparator.

- libstdc++ does not conform to this and always passes the projection result as
  an lvalue.
- libc++ does not conform to this by running the projection twice.
- MSVC STL used to run the projection twice, which was corrected in VS 2022
  17.2.

### Example

```cpp
#include <algorithm>
#include <cstdint>
#include <iomanip>
#include <iostream>
#include <string>

using namespace std::literals;
namespace ranges = std::ranges;

int main()
{
    for (std::cout << " raw   clamped to int8_t   clamped to uint8_t\n";
         int const v : {-129, -128, -1, 0, 42, 127, 128, 255, 256})
        std::cout << std::setw(04) << v
                  << std::setw(20) << ranges::clamp(v, INT8_MIN, INT8_MAX)
                  << std::setw(21) << ranges::clamp(v, 0, UINT8_MAX) << '\n';
    std::cout << '\n';

    // Projection function
    const auto stoi = [](std::string s) { return std::stoi(s); };

    // Same as above, but with strings
    for (std::string const v : {"-129", "-128", "-1", "0", "42",
                                "127", "128", "255", "256"})
        std::cout << std::setw(04) << v
                  << std::setw(20) << ranges::clamp(v, "-128"s, "127"s, {}, stoi)
                  << std::setw(21) << ranges::clamp(v, "0"s, "255"s, {}, stoi)
                  << '\n';
}
```

Output:

```text
 raw   clamped to int8_t   clamped to uint8_t
-129                -128                    0
-128                -128                    0
  -1                  -1                    0
   0                   0                    0
  42                  42                   42
 127                 127                  127
 128                 127                  128
 255                 127                  255
 256                 127                  255

-129                -128                    0
-128                -128                    0
  -1                  -1                    0
   0                   0                    0
  42                  42                   42
 127                 127                  127
 128                 127                  128
 255                 127                  255
 256                 127                  255
```

### See also

- **ranges::min (C++20)** — returns the smaller of the given values (niebloid)
- **ranges::max (C++20)** — returns the greater of the given values (niebloid)
- **in_range (C++20)** — checks if an integer value is in the range of a given
  integer type (function template)
- **clamp (C++17)** — clamps a value between a pair of boundary values (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/clamp*
