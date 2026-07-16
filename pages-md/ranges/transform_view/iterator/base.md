# std::ranges::transform_view<V,F>::*iterator*<Const>::base

```cpp
constexpr const ranges::iterator_t<Base>& base() const & noexcept;  // (1) (since C++20)
constexpr ranges::iterator_t<Base> base() &&;  // (2) (since C++20)
```

Returns the underlying iterator.

1) Returns a reference to the underlying iterator.

2) Move constructs the result from the underlying iterator.

### Parameters

(none)

### Return value

1) A reference to the underlying iterator.

2) An iterator move constructed from the underlying iterator.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <ranges>

int main()
{
    const auto v = { 0, 1, 2, 3, 4 };
    auto x2 = [](int x) { return x << 1; };
    std::ranges::transform_view tv{ v, x2 };
    std::ostream_iterator<int> ostr{ std::cout, " " };

    std::ranges::copy(v, ostr), std::cout << '\n';
    std::ranges::copy(tv.base(), ostr), std::cout << '\n';
    std::ranges::copy(tv, ostr), std::cout << '\n';
}
```

Output:

```text
0 1 2 3 4
0 1 2 3 4
0 2 4 6 8
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3533 | C++20 | the `const&` overload of `base` returns a copy of the
      underlying iterator | returns a reference
  LWG 3593 | C++20 | the `const&` overload of `base` might not be noexcept |
      made noexcept

---
*Source: https://en.cppreference.com/w/cpp/ranges/transform_view/iterator/base*
