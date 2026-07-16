# std::iterator_traits<std::common_iterator>

```cpp
template< std::input_iterator I, class S >
struct iterator_traits<std::common_iterator<I, S>>;  // (since C++20)
```

Provides the uniform interface to the properties of the `std::common_iterator`
type.

### Member types

- **`iterator_concept`** — `std::forward_iterator_tag`, if `I` models
  `std::forward_iterator`; `std::input_iterator_tag` otherwise.
- **`iterator_category`** — `std::forward_iterator_tag`, if the
  `std::iterator_traits<I>::iterator_category` is valid and denotes a type that
  models `std::derived_from<std::forward_iterator_tag>`;
  `std::input_iterator_tag` otherwise.
- **`value_type`** — `std::iter_value_t<I>`
- **`difference_type`** — `std::iter_difference_t<I>`
- **`pointer`** — `decltype(c.operator->())`, if it is well-formed, where `c` is
  an lvalue of type `const std::common_iterator<I, S>`; `void` otherwise.
- **`reference`** — `std::iter_reference_t<I>`

### Example

```cpp
#include <iterator>
#include <type_traits>
#include <vector>

int main()
{
    std::vector v{1, 2, 3, 4};
    using CV = std::common_iterator<
                    std::counted_iterator<std::vector<int>::iterator>,
                    std::default_sentinel_t>;
    CV i{std::counted_iterator{v.begin(), 3}};
    using TRCV = std::iterator_traits<decltype(i)>;
    static_assert(std::is_same<TRCV::iterator_concept, std::forward_iterator_tag>());
    static_assert(std::is_same<TRCV::iterator_category, std::forward_iterator_tag>());
    static_assert(std::is_same<TRCV::value_type, int>());
    static_assert(std::is_same<TRCV::difference_type,
                  std::vector<int>::difference_type>());
    static_assert(std::is_same<TRCV::pointer, decltype(i.operator->())>());
    static_assert(std::is_same<TRCV::reference, int&>());

    CV s{std::default_sentinel};
    using TRCS = std::iterator_traits<decltype(s)>;
    static_assert(std::is_same<TRCS::iterator_concept, std::forward_iterator_tag>());
    static_assert(std::is_same<TRCS::iterator_category, std::forward_iterator_tag>());
    static_assert(std::is_same<TRCS::value_type, int>());
    static_assert(std::is_same<TRCS::difference_type,
                  std::vector<int>::difference_type>());
    static_assert(std::is_same<TRCS::pointer, decltype(s.operator->())>());
    static_assert(std::is_same<TRCS::reference, int&>());
}
```

### See also

- **iterator_traits** — provides uniform interface to the properties of an
  iterator (class template)
- **std::incrementable_traits<std::common_iterator> (C++20)** — computes the
  associated difference type of the `std::common_iterator` type (class template
  specialization)

---
*Source: https://en.cppreference.com/w/cpp/iterator/common_iterator/iterator_traits*
