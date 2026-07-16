# std::ranges::for_each_n, std::ranges::for_each_n_result

```cpp
Call signature
template< std::input_iterator I, class Proj = std::identity,
          std::indirectly_unary_invocable<std::projected<I, Proj>> Fun >
constexpr for_each_n_result<I, Fun>
    for_each_n( I first, std::iter_difference_t<I> n, Fun f, Proj proj = {});  // (1) (since C++20)
Helper types
template< class I, class F >
using for_each_n_result = ranges::in_fun_result<I, F>;  // (2) (since C++20)
```

1) Applies the given function object `f` to the projected result by `proj` of
   dereferencing each iterator in the range `[``first``,``first + n``)`, in
   order.

If the iterator type is mutable, `f` may modify the elements of the range
through the dereferenced iterator. If `f` returns a result, the result is
ignored. If `n` is less than zero, the behavior is undefined.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first** — iterator denoting the begin of the range to apply the function to
- **n** — the number of elements to apply the function to
- **f** — the function to apply to the projected range `[``first``,``first +
  n``)`
- **proj** — projection to apply to the elements

### Return value

An object `{first + n, std::move(f)}`, where `first + n` may be evaluated as
`std::ranges::next(std::move(first), n)` depending on iterator category.

### Complexity

Exactly `n` applications of `f` and `proj`.

### Possible implementation

```cpp
struct for_each_n_fn
{
    template<std::input_iterator I, class Proj = std::identity,
             std::indirectly_unary_invocable<std::projected<I, Proj>> Fun>
    constexpr for_each_n_result<I, Fun>
        operator()(I first, std::iter_difference_t<I> n, Fun fun, Proj proj = Proj{}) const
    {
        for (; n-- > 0; ++first)
            std::invoke(fun, std::invoke(proj, *first));
        return {std::move(first), std::move(fun)};
    }
};

inline constexpr for_each_n_fn for_each_n {};
```

### Example

```cpp
#include <algorithm>
#include <array>
#include <iostream>
#include <ranges>
#include <string_view>

struct P
{
    int first;
    char second;
    friend std::ostream& operator<<(std::ostream& os, const P& p)
    {
        return os << '{' << p.first << ",'" << p.second << "'}";
    }
};

auto print = [](std::string_view name, auto const& v)
{
    std::cout << name << ": ";
    for (auto n = v.size(); const auto& e : v)
        std::cout << e << (--n ? ", " : "\n");
};

int main()
{
    std::array a {1, 2, 3, 4, 5};
    print("a", a);
    // Negate first three numbers:
    std::ranges::for_each_n(a.begin(), 3, [](auto& n) { n *= -1; });
    print("a", a);

    std::array s { P{1,'a'}, P{2, 'b'}, P{3, 'c'}, P{4, 'd'} };
    print("s", s);
    // Negate data members 'P::first' using projection:
    std::ranges::for_each_n(s.begin(), 2, [](auto& x) { x *= -1; }, &P::first);
    print("s", s);
    // Capitalize data members 'P::second' using projection:
    std::ranges::for_each_n(s.begin(), 3, [](auto& c) { c -= 'a'-'A'; }, &P::second);
    print("s", s);
}
```

Output:

```text
a: 1, 2, 3, 4, 5
a: -1, -2, -3, 4, 5
s: {1,'a'}, {2,'b'}, {3,'c'}, {4,'d'}
s: {-1,'a'}, {-2,'b'}, {3,'c'}, {4,'d'}
s: {-1,'A'}, {-2,'B'}, {3,'C'}, {4,'d'}
```

### See also

- **range-`for` loop(C++11)** — executes loop over range
- **ranges::for_each (C++20)** — applies a function to a range of elements
  (niebloid)
- **for_each_n (C++17)** — applies a function object to the first N elements of
  a sequence (function template)
- **for_each** — applies a function to a range of elements (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/for_each_n*
