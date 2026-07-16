# std::ranges::elements_view<V,F>::*iterator*<Const>::operator*

```cpp
constexpr decltype(auto) operator*() const;  // (since C++20)
```

Returns the element into `V` the underlying iterator points to.

Effectively returns `/*get-element*/(this->base())`, where for an expression
`e`, `/*get-element*/(e)` is

- `std::get<N>(*e)`, if `ranges::range_reference_t<Base>` is a reference type.
  Otherwise,
- `static_cast<E>(std::get<N>(*e))`, where `E` is
  `std::remove_cv_t<std::tuple_element_t<N, ranges::range_reference_t<Base>>>`.

### Parameters

(none)

### Return value

The current element.

### Notes

`operator->` is not provided.

### Example

```cpp
#include <iostream>
#include <ranges>
#include <string_view>
#include <tuple>

int main()
{
    using T = std::tuple<int, char, std::string_view>;

    const auto il = {
        T{1, 'A', "α"},
        T{2, 'B', "β"},
        T{3, 'C', "γ"},
    };

    const auto view {il | std::views::elements<2>};

    const auto iter {view.begin() + 1};

    std::cout << *iter << '\n';
}
```

Output:

```text
β
```

### See also

- **operator[] (C++20)** — accesses an element by index (public member function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/elements_view/iterator/operator**
