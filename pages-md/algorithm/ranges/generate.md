# std::ranges::generate

```cpp
Call signature
template< std::input_or_output_iterator O, std::sentinel_for<O> S,
          std::copy_constructible F >
requires std::invocable<F&> && std::indirectly_writable<O, std::invoke_result_t<F&>>
constexpr O
    generate( O first, S last, F gen );  // (1) (since C++20)
template< class R, std::copy_constructible F >
requires std::invocable<F&> && ranges::output_range<R, std::invoke_result_t<F&>>
constexpr ranges::borrowed_iterator_t<R>
    generate( R&& r, F gen );  // (2) (since C++20)
```

1) Assigns the result of *successive* invocations of the function object `gen`
   to each element in the range `[``first``,``last``)`.

2) Same as (1), but uses `r` as the range, as if using `ranges::begin(r)` as
   `first` and `ranges::end(r)` as `last`.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — the range of elements to modify
- **r** — the range of elements to modify
- **gen** — the generator function object

### Return value

An output iterator that compares equal to `last`.

### Complexity

Exactly `ranges::distance(first, last)` invocations of `gen()` and assignments.

### Possible implementation

```cpp
struct generate_fn
{
    template<std::input_or_output_iterator O, std::sentinel_for<O> S,
             std::copy_constructible F>
    requires std::invocable<F&> && std::indirectly_writable<O, std::invoke_result_t<F&>>
    constexpr O operator()(O first, S last, F gen) const
    {
        for (; first != last; *first = std::invoke(gen), ++first)
        {}
        return first;
    }

    template<class R, std::copy_constructible F>
    requires std::invocable<F&> && ranges::output_range<R, std::invoke_result_t<F&>>
    constexpr ranges::borrowed_iterator_t<R> operator()(R&& r, F gen) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), std::move(gen));
    }
};

inline constexpr generate_fn generate {};
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
    static std::random_device device;
    static std::mt19937 engine {device()};
    return distr(engine);
}

void iota(auto& v, int n)
{
    std::ranges::generate(v, [&n]() mutable { return n++; });
}

void print(std::string_view comment, const auto& v)
{
    for (std::cout << comment; int i : v)
        std::cout << i << ' ';
    std::cout << '\n';
}

int main()
{
    std::array<int, 8> v;

    std::ranges::generate(v.begin(), v.end(), dice);
    print("dice: ", v);
    std::ranges::generate(v, dice);
    print("dice: ", v);

    iota(v, 1);
    print("iota: ", v);
}
```

Possible output:

```text
dice: 4 3 1 6 6 4 5 5
dice: 4 2 5 3 6 2 6 2
iota: 1 2 3 4 5 6 7 8
```

### See also

- **ranges::generate_n (C++20)** — saves the result of N applications of a
  function (niebloid)
- **ranges::fill (C++20)** — assigns a range of elements a certain value
  (niebloid)
- **ranges::fill_n (C++20)** — assigns a value to a number of elements
  (niebloid)
- **ranges::transform (C++20)** — applies a function to a range of elements
  (niebloid)
- **generate** — assigns the results of successive function calls to every
  element in a range (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/generate*
