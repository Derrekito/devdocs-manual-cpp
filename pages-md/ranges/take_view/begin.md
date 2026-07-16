# std::ranges::take_view<V>::begin

```cpp
constexpr auto begin() requires (!__SimpleView<V>);  // (1) (since C++20)
constexpr auto begin() const requires ranges::range<const V>;  // (2) (since C++20)
```

Returns an iterator to the first element of the `take_view`.

1) Returns a `std::counted_iterator` or a `ranges::iterator_t<V>`.

2) Returns a `std::counted_iterator` or a `ranges::iterator_t<const V>`.

Overload (1) does not participate in overload resolution if `V` is a simple view
(that is, if `V` and `const V` are views with the same iterator and sentinel
types).

### Parameters

(none)

### Return value

The result depends on the concepts satisfied by possibly const-qualified
underlying view type `Base_`, which is `V` (for overload (1)) or `const V` (for
overload (2)).

Let `base_` be the underlying view, `count_` be the number passed to the
constructor (`​0​` if default initialized).

  The underlying view satisfies ... | `random_access_range`
  yes | no
  `sized_range` | yes | `ranges::begin(base_)` |
      `std::counted_iterator(ranges::begin(base_),
      ranges::range_difference_t<Base_>(this->size()))`
  no | `std::counted_iterator(ranges::begin(base_), count_)`

### Example

```cpp
#include <concepts>
#include <forward_list>
#include <iostream>
#include <ranges>
#include <string_view>
#include <type_traits>
using namespace std::literals;

int main()
{
    {
        static constexpr auto v = {"∀x"sv, "∃y"sv, "ε"sv, "δ"sv};
        auto view = std::ranges::take_view(v, 8);
        auto iter = view.begin();
        std::cout << *iter << '\n';
        static_assert(
            std::ranges::sized_range<decltype(v)> and
            std::ranges::random_access_range<decltype(v)> and
            std::is_same_v<decltype(iter), decltype(std::ranges::begin(v))>
        );
    }

    {
        std::forward_list v = {"Ax"sv, "Ey"sv, "p"sv, "q"sv};
        auto view = std::ranges::take_view(v, 8);
        auto iter = view.begin();
        std::cout << *iter << '\n';
        static_assert(
            not std::ranges::sized_range<decltype(v)> and
            not std::ranges::random_access_range<decltype(v)> and
            std::is_same_v<decltype(iter),
                std::counted_iterator<
                    std::forward_list<std::string_view>::iterator>>
        );
    }
}
```

Output:

```text
∀x
Ax
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2393R1 | C++20 | implicit conversions between signed and unsigned
      integer-class types might fail | made explicit

### See also

- **end (C++20)** — returns an iterator or a sentinel to the end (public member
  function)
- **counted_iterator (C++20)** — iterator adaptor that tracks the distance to
  the end of the range (class template)
- **operator== (C++20)** — compares a sentinel with an iterator returned from
  **`take_view::begin`** (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/take_view/begin*
