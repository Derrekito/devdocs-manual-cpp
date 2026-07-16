# std::ranges::get(std::ranges::subrange)

```cpp
template< std::size_t N, class I, class S, ranges::subrange_kind K >
    requires ((N == 0 && std::copyable<I>) || N == 1)
constexpr auto get( const ranges::subrange<I, S, K>& r );  // (1) (since C++20)
template< std::size_t N, class I, class S, ranges::subrange_kind K >
    requires (N < 2)
constexpr auto get( ranges::subrange<I, S, K>&& r );  // (2) (since C++20)
namespace std { using ranges::get; }  // (3) (since C++20)
```

1) Obtains the iterator or sentinel from a `subrange` lvalue (or a const rvalue)
   when `N == 0` or `N == 1`, respectively. It is mainly provided for structured
   binding support.

2) Same as (1), except that it takes a non-const `subrange` rvalue.

3) (1,2) are imported into namespace `std`, which simplifies their usage and
   makes every `subrange` with a copyable iterator a pair-like type.

### Parameters

- **r** — a `subrange`

### Return value

1) An iterator or sentinel copy constructed from the stored one when `N == 0` or
   `N == 1`, respectively.

2) Same as (1), except that the iterator is move constructed if `N == 0` and `I`
   does not satisfy `copyable`.

### Possible implementation

```cpp
template<std::size_t N, class I, class S, std::ranges::subrange_kind K>
    requires ((N == 0 && std::copyable<I>) || N == 1)
constexpr auto get(const std::ranges::subrange<I, S, K>& r)
{
    if constexpr (N == 0)
        return r.begin();
    else
        return r.end();
}

template<std::size_t N, class I, class S, std::ranges::subrange_kind K>
    requires (N < 2)
constexpr auto get(std::ranges::subrange<I, S, K>&& r)
{
    if constexpr (N == 0)
        return r.begin(); // may perform move construction
    else
        return r.end();
}
```

### Example

```cpp
#include <array>
#include <iostream>
#include <iterator>
#include <ranges>

int main()
{
    std::array a{1, -2, 3, -4};

    std::ranges::subrange sub_a{std::next(a.begin()), std::prev(a.end())};

    std::cout << *std::ranges::get<0>(sub_a) << ' ' // == *(begin(a) + 1)
              << *std::get<1>(sub_a) << '\n';       // == *(end(a) - 1)

    *std::get<0>(sub_a) = 42; // OK
//  *std::get<2>(sub_a) = 13; // hard error: index can only be 0 or 1
}
```

Output:

```text
-2 -4
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3589 | C++20 | the overload for const lvalue was ill-formed if `N == 0`
      and `I` does not model `copyable` | it is removed from the overload set

### See also

- **Structured binding (C++17)** — binds the specified names to sub-objects or
  tuple elements of the initializer
- **get(std::tuple) (C++11)** — tuple accesses specified element (function
  template)
- **get(std::pair) (C++11)** — accesses an element of a `pair` (function
  template)
- **get(std::array) (C++11)** — accesses an element of an `array` (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/subrange/get*
