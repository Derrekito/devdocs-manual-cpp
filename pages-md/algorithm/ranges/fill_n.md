# std::ranges::fill_n

```cpp
Call signature
template< class T, std::output_iterator<const T&> O >
constexpr O
    fill_n( O first, std::iter_difference_t<O> n, const T& value );  // (since C++20)
```

Assigns the given `value` to all elements in the range `[``first``,``first +
n``)`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first** — the beginning of the range of elements to modify
- **n** — number of elements to modify
- **value** — the value to be assigned

### Return value

An output iterator that compares equal to `first + n`.

### Complexity

Exactly `n` assignments.

### Possible implementation

```cpp
struct fill_n_fn
{
    template<class T, std::output_iterator<const T&> O>
    constexpr O operator()(O first, std::iter_difference_t<O> n, const T& value) const
    {
        for (std::iter_difference_t<O> i {}; i != n; ++first, ++i)
            *first = value;
        return first;
    }
};

inline constexpr fill_n_fn fill_n {};
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>

void println(const auto& v)
{
    for (const auto& elem : v)
        std::cout << ' ' << elem;
    std::cout << '\n';
}

int main()
{
    constexpr auto n {8};

    std::vector<std::string> v(n, "▓▓░░");
    println(v);

    std::ranges::fill_n(v.begin(), n, "░░▓▓");
    println(v);
}
```

Output:

```text
▓▓░░ ▓▓░░ ▓▓░░ ▓▓░░ ▓▓░░ ▓▓░░ ▓▓░░ ▓▓░░
░░▓▓ ░░▓▓ ░░▓▓ ░░▓▓ ░░▓▓ ░░▓▓ ░░▓▓ ░░▓▓
```

### See also

- **ranges::fill (C++20)** — assigns a range of elements a certain value
  (niebloid)
- **ranges::copy_n (C++20)** — copies a number of elements to a new location
  (niebloid)
- **ranges::generate (C++20)** — saves the result of a function in a range
  (niebloid)
- **ranges::transform (C++20)** — applies a function to a range of elements
  (niebloid)
- **fill_n** — copy-assigns the given value to N elements in a range (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/fill_n*
