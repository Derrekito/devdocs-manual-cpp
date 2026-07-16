# std::ranges::contiguous_range

```cpp
template< class T >
  concept contiguous_range =
    ranges::random_access_range<T> &&
    std::contiguous_iterator<ranges::iterator_t<T>> &&
    requires(T& t) {
      { ranges::data(t) } ->
        std::same_as<std::add_pointer_t<ranges::range_reference_t<T>>>;
    };  // (since C++20)
```

The `contiguous_range` concept is a refinement of `range` for which
`ranges::begin` returns a model of `contiguous_iterator` and the customization
point `ranges::data` is usable.

### Semantic requirements

`T` models `contiguous_range` only if given an expression `e` such that
`decltype((e))` is `T&`, `std::to_address(ranges::begin(e)) == ranges::data(e)`.

### Example

```cpp
#include <array>
#include <deque>
#include <list>
#include <ranges>
#include <set>
#include <valarray>
#include <vector>

template<typename T> concept CR = std::ranges::contiguous_range<T>;

int main()
{
    int a[4];
    static_assert(
            CR<std::vector<int>> and
        not CR<std::vector<bool>> and
        not CR<std::deque<int>> and
            CR<std::valarray<int>> and
            CR<decltype(a)> and
        not CR<std::list<int>> and
        not CR<std::set<int>> and
            CR<std::array<std::list<int>,42>>
    );
}
```

### See also

- **ranges::sized_range (C++20)** — specifies that a range knows its size in
  constant time (concept)
- **ranges::random_access_range (C++20)** — specifies a range whose iterator
  type satisfies `random_access_iterator` (concept)

---
*Source: https://en.cppreference.com/w/cpp/ranges/contiguous_range*
