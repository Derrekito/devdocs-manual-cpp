# std::default_sentinel_t, std::default_sentinel

```cpp
struct default_sentinel_t {};  // (1) (since C++20)
inline constexpr default_sentinel_t default_sentinel{};  // (2) (since C++20)
```

1) `default_sentinel_t` is an empty class type used to denote the end of a
   range. It can be used together with iterator types that know the bound of
   their range (e.g., `std::counted_iterator`).

2) `default_sentinel` is a constant of type `default_sentinel_t`.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <list>

int main()
{
    std::list<int> l{3, 1, 4, 1, 5, 9, 2, 6};

    std::ranges::copy(std::counted_iterator(std::begin(l), 4),
        std::default_sentinel, std::ostream_iterator<int>{std::cout, " "});
    std::cout << '\n';
}
```

Output:

```text
3 1 4 1
```

### See also

- **istream_iterator** — input iterator that reads from `std::basic_istream`
  (class template)
- **istreambuf_iterator** — input iterator that reads from
  `std::basic_streambuf` (class template)
- **counted_iterator (C++20)** — iterator adaptor that tracks the distance to
  the end of the range (class template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/default_sentinel_t*
