# std::ranges::split_view<V,Pattern>::end

```cpp
constexpr auto end() const;  // (since C++20)
```

Returns an `iterator` or a `sentinel` representing the end of the resulting
subrange.

Equivalent to:

```cpp
constexpr auto end()
{
    if constexpr (ranges::common_range<V>)
        return /*iterator*/{*this, ranges::end(base_), {}};
    else
        return /*sentinel*/{*this};
}
```

### Parameters

(none)

### Return value

An `iterator` or a `sentinel`.

### Example

```cpp
#include <iostream>
#include <ranges>
#include <string_view>

int main()
{
    constexpr std::string_view keywords{"bitand bitor bool break"};
    std::ranges::split_view kw{keywords, ' '};
    const auto count = std::ranges::distance(kw.begin(), kw.end());
    std::cout << "Words count: " << count << '\n';
}
```

Output:

```text
Words count: 4
```

### See also

- **begin (C++20)** — returns an iterator to the beginning (public member
  function)
- **end (C++20)** — returns an iterator or a sentinel to the end (public member
  function of `std::ranges::lazy_split_view<V,Pattern>`)
- **ranges::end (C++20)** — returns a sentinel indicating the end of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/split_view/end*
