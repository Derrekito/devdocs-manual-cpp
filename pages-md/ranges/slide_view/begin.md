# std::ranges::slide_view<V>::begin

```cpp
constexpr auto begin()
    requires (!(__simple_view<V> && __slide_caches_nothing<const V>));  // (1) (since C++23)
constexpr auto begin() const
    requires __slide_caches_nothing<const V>;  // (2) (since C++23)
```

Returns an iterator to the first element of the `slide_view`.

1) If `V` models `__slide_caches_first`, equivalent to: `return iterator<false>(
   ranges::begin(base_), ranges::next(ranges::begin(base_), n_ - 1,
   ranges::end(base_)), n_ );` Otherwise, equivalent to: `return
   iterator<false>(ranges::begin(base_), n_);`.

If `V` models `__slide_caches_first` this function caches the result within the
   `slide_view::cached_begin_` for use on subsequent calls. This is necessary to
   provide the amortized constant-time complexity required by the `range`.

2) Equivalent to: `return iterator<true>(ranges::begin(base_), n_);`.

### Parameters

(none)

### Return value

An iterator to the first element of `slide_view`, which points to the `n_`-sized
subrange of the possibly const-qualified underlying view type – `V` for overload
(1) or `const V` for overload (2).

### Example

```cpp
#include <iostream>
#include <ranges>
#include <string_view>
using namespace std::literals;

int main()
{
    static constexpr auto source = {"∀x"sv, "∃y"sv, "ε"sv, "δ"sv};
    auto view{std::ranges::slide_view(source, 2)};
    const auto subrange{*(view.begin())};
    for (std::string_view const s : subrange)
        std::cout << s << ' ';
    std::cout << '\n';
}
```

Output:

```text
∀x ∃y
```

### See also

- **end (C++23)** — returns an iterator or a sentinel to the end (public member
  function)
- **operator== (C++23)** — compares a sentinel with an iterator returned from
  **`slide_view::begin`** (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/slide_view/begin*
