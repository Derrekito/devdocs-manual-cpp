# std::ranges::drop_while_view<V,Pred>::begin

```cpp
constexpr auto begin();  // (since C++20)
```

Returns an iterator to the first element of the view.

Effectively returns `ranges::find_if_not(base_, std::cref(pred()))`, where
`base_` is the underlying view. The behavior is undefined if `*this` does not
store a predicate.

In order to provide the amortized constant time complexity required by the
`range` concept, this function caches the result within the `drop_while_view`
object for use on subsequent calls.

### Parameters

(none)

### Return value

Iterator to the first element of the view.

### Example

```cpp
#include <array>
#include <iostream>
#include <ranges>

int main()
{
    constexpr std::array data{ 0, -1, -2, 3, 1, 4, 1, 5 };

    auto view = std::ranges::drop_while_view{
        data, [](int x) { return x <= 0; }
    };

    std::cout << *view.begin() << '\n';
}
```

Output:

```text
3
```

### See also

- **end (C++20)** — returns an iterator or a sentinel to the end (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/drop_while_view/begin*
