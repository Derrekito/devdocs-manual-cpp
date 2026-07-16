# std::ranges::slide_view<V>::end

```cpp
constexpr auto end()
    requires (!(__simple_view<V> && __slide_caches_nothing<const V>));  // (1) (since C++23)
constexpr auto end() const
    requires __slide_caches_nothing<const V>;  // (2) (since C++23)
```

Returns a sentinel or an iterator representing the end of the `slide_view`.

1) Let `base_` and `n_` be the underlying data members. Equivalent to:

- If `V` models `__slide_caches_nothing`, `return
  iterator<false>(ranges::begin(base_) + ranges::range_difference_t<V>(size()),
  n_);`.
- Otherwise, if `V` models `__slide_caches_last`, `return
  iterator<false>(ranges::prev(ranges::end(base_), n_ - 1,
  ranges::begin(base_)), n_);`.
- Otherwise, if `V` models `common_range`, `return
  iterator<false>(ranges::end(base_), ranges::end(base_), n_);`.
- Otherwise, `return sentinel(ranges::end(base_));`.

If `V` models `__slide_caches_last`, this function caches the result within the
   `slide_view::cached_end_` for use on subsequent calls. This is necessary to
   provide the amortized constant-time complexity required by the `range`.

2) Equivalent to `begin() + ranges::range_difference_t<const V>(size())`.

### Parameters

(none)

### Return value

A sentinel or an iterator representing the end of the `slide_view`.

### Example

```cpp
#include <iostream>
#include <ranges>

int main()
{
    static constexpr auto source = {'A', 'B', 'C', 'D'};

    for (const auto subrange: source | std::views::slide(3))
    {
        std::cout << "[ ";
        for (auto it = subrange.begin(); it != subrange.end(); ++it)
            std::cout << *it << ' ';
        std::cout << "]\n";
    }
}
```

Output:

```text
[ A B C ]
[ B C D ]
```

### See also

- **begin (C++23)** — returns an iterator to the beginning (public member
  function)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)
- **ranges::end (C++20)** — returns a sentinel indicating the end of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/slide_view/end*
