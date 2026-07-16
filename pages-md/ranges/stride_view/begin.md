# std::ranges::stride_view<V>::begin

```cpp
constexpr auto begin() requires (!__simple_view<V>);  // (1) (since C++23)
constexpr auto begin() const requires ranges::range<const V>;  // (2) (since C++23)
```

Returns an iterator to the first element of the `stride_view`.

1) Equivalent to `return iterator<false>(this, ranges::begin(base_));`.

2) Equivalent to `return iterator<true>(this, ranges::begin(base_));`.

Overload (1) does not participate in overload resolution if `V` is a simple view
(that is, if `V` and `const V` are views with the same iterator and sentinel
types).

### Parameters

(none)

### Return value

Iterator to the first element of the view.

### Example

A link to test: Compiler Explorer.

```cpp
#include <print>
#include <ranges>

int main()
{
    constexpr auto v = {'A', 'B', 'C'};
    const auto x = v | std::views::stride(2);
    const auto y = v | std::views::reverse | std::views::stride(2);
    const auto z = v | std::views::stride(2) | std::views::reverse;
    std::println("{} {} {}", *x.begin(), *y.begin(), *z.begin());
}
```

Output:

```text
A C C
```

### See also

- **end (C++23)** — returns an iterator or a sentinel to the end (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/stride_view/begin*
