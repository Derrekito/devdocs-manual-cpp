# std::ranges::bidirectional_range

```cpp
template< class T >
  concept bidirectional_range =
    ranges::forward_range<T> && std::bidirectional_iterator<ranges::iterator_t<T>>;  // (since C++20)
```

The `bidirectional_range` concept is a refinement of `range` for which
`ranges::begin` returns a model of `bidirectional_iterator`.

### Example

```cpp
#include <forward_list>
#include <list>
#include <ranges>
#include <set>
#include <unordered_set>

int main()
{
    static_assert(
            std::ranges::bidirectional_range<std::set<int>> and
        not std::ranges::bidirectional_range<std::unordered_set<int>> and
            std::ranges::bidirectional_range<std::list<int>> and
        not std::ranges::bidirectional_range<std::forward_list<int>>
    );
}
```

### See also

- **ranges::forward_range (C++20)** — specifies a range whose iterator type
  satisfies `forward_iterator` (concept)
- **ranges::random_access_range (C++20)** — specifies a range whose iterator
  type satisfies `random_access_iterator` (concept)

---
*Source: https://en.cppreference.com/w/cpp/ranges/bidirectional_range*
