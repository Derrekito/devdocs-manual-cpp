# std::tuple_element<std::ranges::subrange>

```cpp
template< class I, class S, ranges::subrange_kind K >
struct tuple_element<0, ranges::subrange<I, S, K>>;  // (1) (since C++20)
template< class I, class S, ranges::subrange_kind K >
struct tuple_element<0, const ranges::subrange<I, S, K>>;  // (2) (since C++20)
template< class I, class S, ranges::subrange_kind K >
struct tuple_element<1, ranges::subrange<I, S, K>>;  // (3) (since C++20)
template< class I, class S, ranges::subrange_kind K >
struct tuple_element<1, const ranges::subrange<I, S, K>>;  // (4) (since C++20)
```

The partial specializations of `std::tuple_element` for `std::ranges::subrange`
provide compile-time access to the iterator or sentinel type of a `subrange`,
using tuple-like syntax. They are provided for structured binding support.

1,2) Obtains the iterator type, i.e. `I`.

3,4) Obtains the sentinel type, i.e. `S`.

### Member types

- **`type`** — (1,2) `I` (3,4) `S`

### Notes

As `get` functions for `subrange` return iterators and sentinels by value,
`const` qualifier is not added to the result types when the `subrange` is
const-qualified (but not volatile-qualified).

If the `subrange` is volatile-qualified, the result types are also
volatile-qualified because the partial specialization for volatile or const
volatile types are used. Such usage is deprecated.

### Example

```cpp
#include <iterator>
#include <list>
#include <ranges>
#include <type_traits>

int main()
{
    std::list<int> list{3, 1, 4, 1, 5, 9, 2, 6};

    std::ranges::subrange subrange
    {
        std::counted_iterator{std::begin(list), 4},
        std::default_sentinel
    };

    static_assert(
        std::is_same_v<
            std::tuple_element_t<0, decltype(subrange)>,
            // implementation-defined type:
            std::counted_iterator<std::_List_iterator<int>>
            >);

    static_assert(
        std::is_same_v<
            std::tuple_element_t<1, decltype(subrange)>,
            std::default_sentinel_t
            >);
}
```

### See also

- **Structured binding (C++17)** — binds the specified names to sub-objects or
  tuple elements of the initializer
- **tuple_element (C++11)** — obtains the element types of a tuple-like type
  (class template)
- **std::tuple_size<std::ranges::subrange> (C++20)** — obtains the number of
  components of a `std::ranges::subrange` (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/ranges/subrange/tuple_element*
