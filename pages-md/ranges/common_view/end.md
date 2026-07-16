# std::ranges::common_view<V>::end

```cpp
constexpr auto end();  // (1) (since C++20)
constexpr auto end() const requires ranges::range<const V>;  // (2) (since C++20)
```

1) Returns an iterator representing the end of the `common_view`, that is:

- `ranges::begin(base_) + ranges::size(base_)`, if both
  `ranges::random_access_range<V>` and `ranges::sized_range<V>` are satisfied,
- `std::common_iterator<ranges::iterator_t<V>,
  ranges::sentinel_t<V>>(ranges::end(base_))` otherwise.

Here `base_` (the name is for exposition purposes only) is the underlying view.

2) Same as (1), but `V` is const-qualified.

### Parameters

(none)

### Return value

An iterator representing the end of the underlying view.

### Example

```cpp
#include <iostream>
#include <numeric>
#include <ranges>

int main()
{
    constexpr int n{4};

    constexpr auto v1 = std::views::iota(1)
                      | std::views::take(n)
                      | std::views::common
                      ;
    constexpr auto v2 = std::views::iota(2)
                      | std::views::take(n)
                      ;
    const int product = std::inner_product(v1.begin(), v1.end(),
                                           v2.begin(),
                                           0);
    std::cout << product << '\n';
}
```

Output:

```text
40
```

### See also

- **begin (C++20)** — returns an iterator to the beginning (public member
  function)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)
- **ranges::end (C++20)** — returns a sentinel indicating the end of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/common_view/end*
