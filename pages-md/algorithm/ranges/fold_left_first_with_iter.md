# std::ranges::fold_left_first_with_iter, std::ranges::fold_left_first_with_iter_result

```cpp
Call signature
template< std::input_iterator I, std::sentinel_for<I> S,
          __indirectly_binary_left_foldable<std::iter_value_t<I>, I> F >
requires std::constructible_from<
             std::iter_value_t<I>, std::iter_reference_t<I>>
constexpr /* see description */
    fold_left_first_with_iter( I first, S last, F f );  // (1) (since C++23)
template< ranges::input_range R,
          __indirectly_binary_left_foldable<
              ranges::range_value_t<R>, ranges::iterator_t<R>> F >
requires std::constructible_from<
             ranges::range_value_t<R>, ranges::range_reference_t<R>>
constexpr /* see description */
    fold_left_first_with_iter( R&& r, F f );  // (2) (since C++23)
Helper concepts
template< class F, class T, class I >
concept __indirectly_binary_left_foldable = /* see description */;  // (3) (exposition only*)
Helper class template
template< class I, class T >
using fold_left_first_with_iter_result = ranges::in_value_result<I, T>;  // (4) (since C++23)
```

Left-folds the elements of given range, that is, returns the result of
evaluation of the chain expression:
`f(f(f(f(x1, x2), x3), ...), xn)`, where `x1`, `x2`, ..., `xn` are elements of
the range.

Informally, `ranges::fold_left_first_with_iter` behaves like `std::accumulate`'s
overload that accepts a binary predicate, except that the `*first` is used
internally as an initial element.

The behavior is undefined if `[``first``,``last``)` is not a valid range.

1) The range is `[``first``,``last``)`.

2) Same as (1), except that uses `r` as the range, as if by using
   `ranges::begin(r)` as `first` and `ranges::end(r)` as `last`.

3) Equivalent to: Helper concepts template< class F, class T, class I, class U >
   concept /*indirectly-binary-left-foldable-impl*/ = std::movable<T> &&
   std::movable<U> && std::convertible_to<T, U> && std::invocable<F&, U,
   std::iter_reference_t<I>> && std::assignable_from<U&,
   std::invoke_result_t<F&, U, std::iter_reference_t<I>>>; (3A) (exposition
   only*) template< class F, class T, class I > concept
   /*indirectly-binary-left-foldable*/ = std::copy_constructible<F> &&
   std::indirectly_readable<I> && std::invocable<F&, T,
   std::iter_reference_t<I>> && std::convertible_to<std::invoke_result_t<F&, T,
   std::iter_reference_t<I>>, std::decay_t<std::invoke_result_t<F&, T,
   std::iter_reference_t<I>>>> && /*indirectly-binary-left-foldable-impl*/<F, T,
   I, std::decay_t<std::invoke_result_t<F&, T, std::iter_reference_t<I>>>>; (3B)
   (exposition only*)

4) The return type alias. See Return value section for details.

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — the range of elements to fold
- **r** — the range of elements to fold
- **f** — the binary function object

### Return value

Let `U` be `decltype(ranges::fold_left(std::move(first), last,
std::iter_value_t<I>(*first), f))`.

1) An object of type `ranges::fold_left_first_with_iter_result<I,
std::optional<U>>`.

- The member `ranges::in_value_result::in` holds an iterator to the end of the
  range.
- The member `ranges::in_value_result::value` holds the result of the left-fold
  of given range over `f`.

If the range is empty, the return value is `{std::move(first),
std::optional<U>()}`.

2) Same as (1) except that the return type is
   `ranges::fold_left_first_with_iter_result<ranges::borrowed_iterator_t<R>,
   std::optional<U>>`.

### Possible implementations

```cpp
class fold_left_first_with_iter_fn
{
    template<class O, class I, class S, class F>
    constexpr auto impl(I&& first, S&& last, F f) const
    {
        using U = decltype(
            ranges::fold_left(std::move(first), last, std::iter_value_t<I>(*first), f)
        );
        using Ret = ranges::fold_left_first_with_iter_result<O, std::optional<U>>;
        if (first == last)
            return Ret{std::move(first), std::optional<U>()};
        std::optional<U> init(std::in_place, *first);
        for (++first; first != last; ++first)
            *init = std::invoke(f, std::move(*init), *first);
        return Ret{std::move(first), std::move(init)};
    }

public:
    template<std::input_iterator I, std::sentinel_for<I> S,
             __indirectly_binary_left_foldable<std::iter_value_t<I>, I> F>
    requires std::constructible_from<std::iter_value_t<I>, std::iter_reference_t<I>>
    constexpr auto operator()(I first, S last, F f) const
    {
        return impl<I>(std::move(first), std::move(last), std::ref(f));
    }

    template<ranges::input_range R, __indirectly_binary_left_foldable<
        ranges::range_value_t<R>, ranges::iterator_t<R>> F>
    requires
        std::constructible_from<ranges::range_value_t<R>, ranges::range_reference_t<R>>
    constexpr auto operator()(R&& r, F f) const
    {
        return impl<ranges::borrowed_iterator_t<R>>(
            ranges::begin(r), ranges::end(r), std::ref(f)
        );
    }
};

inline constexpr fold_left_first_with_iter_fn fold_left_first_with_iter;
```

### Complexity

Exactly `ranges::distance(first, last) - 1` (assuming the range is not empty)
applications of the function object `f`.

### Notes

The following table compares all constrained folding algorithms:

  Fold function template | Starts from | Initial value | Return type
  `ranges::fold_left` | left | `init` | `U`
  `ranges::fold_left_first` | left | first element | `std::optional<U>`
  `ranges::fold_right` | right | `init` | `U`
  `ranges::fold_right_last` | right | last element | `std::optional<U>`
  `ranges::fold_left_with_iter` | left | `init` | (1)
      `ranges::in_value_result<I, U>` (2) `ranges::in_value_result<BR, U>`,
      where `BR` is `ranges::borrowed_iterator_t<R>`
  `ranges::fold_left_first_with_iter` | left | first element | (1)
      `ranges::in_value_result<I, std::optional<U>>` (2)
      `ranges::in_value_result<BR, std::optional<U>>` where `BR` is
      `ranges::borrowed_iterator_t<R>`

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_fold` | 202207L | (C++23) | `std::ranges` fold algorithms

### Example

```cpp
#include <algorithm>
#include <cassert>
#include <functional>
#include <iostream>
#include <ranges>
#include <utility>
#include <vector>

int main()
{
    std::vector<int> v {1, 2, 3, 4, 5, 6, 7, 8};

    auto sum = std::ranges::fold_left_first_with_iter
    (
        v.begin(), v.end(), std::plus<int>()
    );
    std::cout << "sum: " << sum.value.value() << '\n';
    assert(sum.in == v.end());

    auto mul = std::ranges::fold_left_first_with_iter(v, std::multiplies<int>());
    std::cout << "mul: " << mul.value.value() << '\n';
    assert(mul.in == v.end());

    // get the product of the std::pair::second of all pairs in the vector:
    std::vector<std::pair<char, float>> data {{'A', 2.f}, {'B', 3.f}, {'C', 7.f}};
    auto sec = std::ranges::fold_left_first_with_iter
    (
        data | std::ranges::views::values, std::multiplies<>()
    );
    std::cout << "sec: " << sec.value.value() << '\n';

    // use a program defined function object (lambda-expression):
    auto lambda = [](int x, int y) { return x + y + 2; };
    auto val = std::ranges::fold_left_first_with_iter(v, lambda);
    std::cout << "val: " << val.value.value() << '\n';
    assert(val.in == v.end());
}
```

Output:

```text
sum: 36
mul: 40320
sec: 42
val: 50
```

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

- **ranges::fold_left (C++23)** — left-folds a range of elements (niebloid)
- **ranges::fold_left_first (C++23)** — left-folds a range of elements using the
  first element as an initial value (niebloid)
- **ranges::fold_right (C++23)** — right-folds a range of elements (niebloid)
- **ranges::fold_right_last (C++23)** — right-folds a range of elements using
  the last element as an initial value (niebloid)
- **ranges::fold_left_with_iter (C++23)** — left-folds a range of elements, and
  returns a pair (iterator, value) (niebloid)
- **accumulate** — sums up or folds a range of elements (function template)
- **reduce (C++17)** — similar to `std::accumulate`, except out of order
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/fold_left_first_with_iter*
