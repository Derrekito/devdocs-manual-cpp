# std::ranges::take_view<V>::size

```cpp
constexpr auto size() requires ranges::sized_range<V>;  // (1) (since C++20)
constexpr auto size() const requires ranges::sized_range<const V>;  // (2) (since C++20)
```

Returns the number of elements, which is the smaller of the count passed to the
constructor and the size of the underlying view.

Let `base_` be the underlying view, `count_` be the number passed to the
constructor (`​0​` if default constructed). Equivalent to

```cpp
auto n = ranges::size(base_);
return ranges::min(n, static_cast<decltype(n)>(count_));
```

### Parameters

(none)

### Return value

The number of elements.

### Example

```cpp
#include <iostream>
#include <ranges>

int main()
{
    constexpr int arr[]{1, 2, 3};

    for (int i = 0; i != 6; ++i)
    {
        const auto tv = std::ranges::take_view{arr, i};
        std::cout << tv.size() << ' ';
    }
    std::cout << '\n';
}
```

Output:

```text
0 1 2 3 3 3
```

### See also

- **ranges::size (C++20)** — returns an integer equal to the size of a range
  (customization point object)
- **ranges::ssize (C++20)** — returns a signed integer equal to the size of a
  range (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/take_view/size*
