# std::ranges::drop_view<V>::end

```cpp
constexpr auto end() requires (!__SimpleView<V>);  // (1) (since C++20)
constexpr auto end() const requires ranges::range<const V>;  // (2) (since C++20)
```

Returns a sentinel or an iterator representing the end of the `drop_view`.

Effectively returns `ranges::end(base_)`, where `base_` is the underlying view.

### Parameters

(none)

### Return value

A sentinel or an iterator representing the end of the view.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <ranges>

int main()
{
    constexpr char url[]{"https://cppreference.com"};

    const auto p = std::distance(std::ranges::begin(url), std::ranges::find(url, '/'));
    auto site = std::ranges::drop_view{url, p + 2}; // drop the prefix "https://"

    for (auto it = site.begin(); it != site.end(); ++it)
        std::cout << *it; //                ^^^
    std::cout << '\n';
}
```

Output:

```text
cppreference.com
```

### See also

- **begin (C++20)** — returns an iterator to the beginning (public member
  function)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)
- **ranges::end (C++20)** — returns a sentinel indicating the end of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/drop_view/end*
