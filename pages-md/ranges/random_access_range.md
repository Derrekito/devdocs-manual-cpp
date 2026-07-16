# std::ranges::random_access_range

```cpp
template< class T >
  concept random_access_range =
    ranges::bidirectional_range<T> && std::random_access_iterator<ranges::iterator_t<T>>;  // (since C++20)
```

The `random_access_range` concept is a refinement of `range` for which
`ranges::begin` returns a model of `random_access_iterator`.

### Example

```cpp
#include <array>
#include <deque>
#include <list>
#include <ranges>
#include <set>
#include <valarray>
#include <vector>

template<typename T> concept RAR = std::ranges::random_access_range<T>;

int main()
{
    int a[4];
    static_assert(
            RAR<std::vector<int>> and
            RAR<std::vector<bool>> and
            RAR<std::deque<int>> and
            RAR<std::valarray<int>> and
            RAR<decltype(a)> and
        not RAR<std::list<int>> and
        not RAR<std::set<int>> and
            RAR<std::array<std::list<int>,42>>
    );
}
```

### See also

- **ranges::sized_range (C++20)** — specifies that a range knows its size in
  constant time (concept)
- **ranges::contiguous_range (C++20)** — specifies a range whose iterator type
  satisfies `contiguous_iterator` (concept)

---
*Source: https://en.cppreference.com/w/cpp/ranges/random_access_range*
