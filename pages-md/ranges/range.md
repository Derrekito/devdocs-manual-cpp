# std::ranges::range

```cpp
template< class T >
concept range = requires( T& t ) {
  ranges::begin(t); // equality-preserving for forward iterators
  ranges::end  (t);
};  // (since C++20)
```

The `range` concept defines the requirements of a type that allows iteration
over its elements by providing an iterator and sentinel that denote the elements
of the range.

### Semantic requirements

Given an expression `E` such that decltype((E)) is `T`, `T` models `range` only
if

- `[``ranges::begin(E)``,``ranges::end(E)``)` denotes a range, and
- both `ranges::begin(E)` and `ranges::end(E)` are amortized constant time and
  do not alter the value of `E` in a manner observable to equality-preserving
  expressions, and
- if the type of `ranges::begin(E)` models `forward_iterator`,
  `ranges::begin(E)` is equality-preserving (in other words, forward iterators
  support multi-pass algorithms).

### Notes

A typical `range` class only needs to provide two functions:

1. A member function `begin()` whose return type models
   `input_or_output_iterator`.
1. A member function `end()` whose return type models `sentinel_for``<It>`,
   where `It` is the return type of `begin()`.

Alternatively, they can be non-member functions, to be found by
argument-dependent lookup.

### Example

```cpp
#include <iostream>
#include <ranges>
#include <vector>

template<typename T>
struct range_t : private T
{
    using T::begin, T::end; /* ... */
};
static_assert(std::ranges::range<range_t<std::vector<int>>>);

template<typename T>
struct scalar_t
{
    T t {}; /* no begin/end */
};
static_assert(not std::ranges::range<scalar_t<int>>);

int main()
{
    if constexpr (range_t<std::vector<int>> r; std::ranges::range<decltype(r)>)
        std::cout << "r is a range\n";

    if constexpr (scalar_t<int> s; not std::ranges::range<decltype(s)>)
        std::cout << "s is not a range\n";
}
```

Output:

```text
r is a range
s is not a range
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3915 | C++20 | `ranges::begin(t)` and `ranges::end(t)` did not require
      implicit expression variations | removed the redundant description

---
*Source: https://en.cppreference.com/w/cpp/ranges/range*
