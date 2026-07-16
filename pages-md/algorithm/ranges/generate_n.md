# std::ranges::generate_n

```cpp
Call signature
template< std::input_or_output_iterator O, std::copy_constructible F >
requires std::invocable<F&> && std::indirectly_writable<O, std::invoke_result_t<F&>>
constexpr O
    generate_n( O first, std::iter_difference_t<O> n, F gen );  // (since C++20)
```

Assigns the result of *successive* invocations of the function object `gen` to
each element in the range `[``first``,``first + n``)`, if `0 < n`. Does nothing
otherwise.

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
- **gen** — the generator function object.

### Return value

Iterator one past the last element assigned if `0 < count`, `first` otherwise.

### Complexity

Exactly `n` invocations of `gen()` and assignments.

### Possible implementation

```cpp
struct generate_n_fn
{
    template<std::input_or_output_iterator O, std::copy_constructible F>
    requires std::invocable<F&> && std::indirectly_writable<O, std::invoke_result_t<F&>>
    constexpr O operator()(O first, std::iter_difference_t<O> n, F gen) const
    {
        for (; n-- > 0; *first = std::invoke(gen), ++first)
        {}
        return first;
    }
};

inline constexpr generate_n_fn generate_n {};
```

### Example

```cpp
#include <algorithm>
#include <array>
#include <iostream>
#include <random>
#include <string_view>

auto dice()
{
    static std::uniform_int_distribution<int> distr {1, 6};
    static std::random_device engine;
    static std::mt19937 noise {engine()};
    return distr(noise);
}

void print(const auto& v, std::string_view comment)
{
    for (int i : v)
        std::cout << i << ' ';
    std::cout << '(' << comment << ")\n";
}

int main()
{
    std::array<int, 8> v;

    std::ranges::generate_n(v.begin(), v.size(), dice);
    print(v, "dice");

    std::ranges::generate_n(v.begin(), v.size(), [n {0}] mutable { return n++; });
    // same effect as std::iota(v.begin(), v.end(), 0);
    print(v, "iota");
}
```

Possible output:

```text
5 5 2 2 6 6 3 5 (dice)
0 1 2 3 4 5 6 7 (iota)
```

### See also

- **ranges::generate (C++20)** — saves the result of a function in a range
  (niebloid)
- **ranges::fill (C++20)** — assigns a range of elements a certain value
  (niebloid)
- **ranges::fill_n (C++20)** — assigns a value to a number of elements
  (niebloid)
- **ranges::transform (C++20)** — applies a function to a range of elements
  (niebloid)
- **generate_n** — assigns the results of successive function calls to N
  elements in a range (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/generate_n*
