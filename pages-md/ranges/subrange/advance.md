# std::ranges::subrange<I,S,K>::advance

```cpp
constexpr subrange& advance( std::iter_difference_t<I> n );  // (since C++20)
```

If `n >= 0`, increments the stored iterator for `n` times, or until it is equal
to the stored sentinel, whichever comes first. Otherwise, decrements the stored
iterator for `-n` times.

The stored size, if any, is adjusted accordingly (increased by `-n` if `n < 0`,
decreased by `m` otherwise, where `m` is the number of increments actually
applied to the iterator).

The behavior is undefined if

- `I` does not model `bidirectional_iterator` and `n < 0`, or
- the stored iterator is decremented after becoming a non-decrementable value.

### Parameters

- **n** — number of maximal increments of the iterator

### Return value

`*this`

### Complexity

Generally `min(n, size())` increments or `-n` decrements on the iterator, when
`n >= 0` or `n < 0` respectively.

Constant if `I` models `random_access_iterator`, and either `n < 0` or
`std::sized_sentinel_for<S, I>` is modeled.

### Notes

The stored size presents if and only if `K == ranges::subrange_kind::sized` but
`std::sized_sentinel_for<S, I>` is not satisfied.

### Example

```cpp
#include <algorithm>
#include <array>
#include <iostream>
#include <iterator>
#include <ranges>

void print(auto name, auto const sub) {
    std::cout << name << ".size() == " << sub.size() << "; { ";
    std::ranges::for_each(sub, [](int x) { std::cout << x << ' '; });
    std::cout << "}\n";
};

int main()
{
    std::array arr{1,2,3,4,5,6,7};
    std::ranges::subrange sub{ std::next(arr.begin()), std::prev(arr.end()) };
    print("1) sub", sub);
    print("2) sub", sub.advance(3));
    print("3) sub", sub.advance(-2));
}
```

Output:

```text
1) sub.size() == 5; { 2 3 4 5 6 }
2) sub.size() == 2; { 5 6 }
3) sub.size() == 4; { 3 4 5 6 }
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3433 | C++20 | the specification mishandled the cases when `n < 0` |
      corrected

### See also

- **next (C++20)** — obtains a copy of the `subrange` with its iterator advanced
  by a given distance (public member function)
- **prev (C++20)** — obtains a copy of the `subrange` with its iterator
  decremented by a given distance (public member function)
- **advance** — advances an iterator by given distance (function template)
- **ranges::advance (C++20)** — advances an iterator by given distance or to a
  given bound (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/ranges/subrange/advance*
