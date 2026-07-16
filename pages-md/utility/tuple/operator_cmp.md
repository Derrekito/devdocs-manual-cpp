# operator==,!=,<,<=,>,>=,<=>(std::tuple)

```cpp
template< class... TTypes, class... UTypes >
bool operator==( const std::tuple<TTypes...>& lhs,
                 const std::tuple<UTypes...>& rhs );  // (since C++11) (until C++14)
template< class... TTypes, class... UTypes >
constexpr bool operator==( const std::tuple<TTypes...>& lhs,
                           const std::tuple<UTypes...>& rhs );  // (since C++14)
template< class... TTypes, class... UTypes >
bool operator!=( const std::tuple<TTypes...>& lhs,
                 const std::tuple<UTypes...>& rhs );  // (since C++11) (until C++14)
template< class... TTypes, class... UTypes >
constexpr bool operator!=( const std::tuple<TTypes...>& lhs,
                           const std::tuple<UTypes...>& rhs );  // (since C++14) (until C++20)
template< class... TTypes, class... UTypes >
bool operator<( const std::tuple<TTypes...>& lhs,
                const std::tuple<UTypes...>& rhs );  // (since C++11) (until C++14)
template< class... TTypes, class... UTypes >
constexpr bool operator<( const std::tuple<TTypes...>& lhs,
                          const std::tuple<UTypes...>& rhs );  // (since C++14) (until C++20)
template< class... TTypes, class... UTypes >
bool operator<=( const std::tuple<TTypes...>& lhs,
                 const std::tuple<UTypes...>& rhs );  // (since C++11) (until C++14)
template< class... TTypes, class... UTypes >
constexpr bool operator<=( const std::tuple<TTypes...>& lhs,
                           const std::tuple<UTypes...>& rhs );  // (since C++14) (until C++20)
template< class... TTypes, class... UTypes >
bool operator>( const std::tuple<TTypes...>& lhs,
                const std::tuple<UTypes...>& rhs );  // (since C++11) (until C++14)
template< class... TTypes, class... UTypes >
constexpr bool operator>( const std::tuple<TTypes...>& lhs,
                          const std::tuple<UTypes...>& rhs );  // (since C++14) (until C++20)
template< class... TTypes, class... UTypes >
bool operator>=( const std::tuple<TTypes...>& lhs,
                 const std::tuple<UTypes...>& rhs );  // (since C++11) (until C++14)
template< class... TTypes, class... UTypes >
constexpr bool operator>=( const std::tuple<TTypes...>& lhs,
                           const std::tuple<UTypes...>& rhs );  // (since C++14) (until C++20)
template< class... TTypes, class... UTypes >
constexpr std::common_comparison_category_t<
    synth-three-way-result<TTypes, Elems>...>
    operator<=>( const std::tuple<TTypes...>& lhs,
                 const std::tuple<UTypes...>& rhs );  // (7) (since C++20)
template< class... TTypes, tuple-like UTuple >
constexpr bool operator==( const tuple<TTypes...>& lhs, const UTuple& rhs );  // (8) (since C++23)
template< class... TTypes, tuple-like UTuple >
constexpr std::common_comparison_category_t<
    synth-three-way-result<TTypes, /* Elems */>...>
    operator<=>( const tuple<TTypes...>& lhs, const UTuple& rhs );  // (9) (since C++23)
```

1,2) Compares every element of the tuple `lhs` with the corresponding element of
   the tuple `rhs` by `operator==`.

   1) Returns `true` if all pairs of corresponding elements are equal.

   2) Returns `!(lhs == rhs)`.

If `sizeof...(TTypes)` does not equal `sizeof...(UTypes)`, or `std::get<i>(lhs)
   == std::get<i>(rhs)` is not a valid expression returning a type that is
   convertible to `bool`(until C++23) for any `i` in
   `[``​0​``,``sizeof...(Types)``)`, the program is ill-formed.

If decltype(std::get<i>(lhs) == std::get<i>(rhs)) does not model
   `boolean-testable` for any `i` in `[``​0​``,``sizeof...(Types)``)`, the
   behavior is undefined. (since C++23)

3-6) Compares `lhs` and `rhs` lexicographically by `operator<`, that is,
   compares the first elements, if they are equivalent, compares the second
   elements, if those are equivalent, compares the third elements, and so on.

   3) For empty tuples, returns `false`. For non-empty tuples, the effect is
      equivalent to `if (std::get<0>(lhs) < std::get<0>(rhs)) return true; if
      (std::get<0>(rhs) < std::get<0>(lhs)) return false; if (std::get<1>(lhs) <
      std::get<1>(rhs)) return true; if (std::get<1>(rhs) < std::get<1>(lhs))
      return false; ... return std::get<N - 1>(lhs) < std::get<N - 1>(rhs);`

   4) Returns `!(rhs < lhs)`.

   5) Returns `rhs < lhs`.

   6) Returns `!(lhs < rhs)`.

If `sizeof...(TTypes)` does not equal `sizeof...(UTypes)`, or `std::get<i>(lhs)
   < std::get<i>(rhs)` is not a valid expression returning a type that is
   convertible to `bool` for any `i` in `[``​0​``,``sizeof...(Types)``)`, the
   program is ill-formed.
7) Compares `lhs` and `rhs` lexicographically by `synth-three-way`, that is,
compares the first elements, if they are equivalent, compares the second
elements, if those are equivalent, compares the third elements, and so on.

- For empty tuples, returns `std::strong_ordering::equal`.
- For non-empty tuples, the effect is equivalent to

   `if (auto c = synth-three-way(std::get<0>(lhs), std::get<0>(rhs)); c != 0)
      return c; if (auto c = synth-three-way(std::get<1>(lhs),
      std::get<1>(rhs)); c != 0) return c; ... return synth-three-way(std::get<N
      - 1>(lhs), std::get<N - 1>(rhs));`

8) Same as (1), except that `rhs` is a `tuple-like` object, and the number of
   elements of `rhs` is determined by `std::tuple_size_v<UTuple>` instead. This
   overload can only be found via argument-dependent lookup.

9) Same as (7), except that `rhs` is a `tuple-like` object. `/* Elems */`
   denotes the pack of types `std::tuple_element_t<i, UTuple>` for each `i` in
   `[``​0​``,``std::tuple_size_v<UTuple>``)` in increasing order. This overload
   can only be found via argument-dependent lookup.

All comparison operators are short-circuited; they do not access tuple elements
beyond what is necessary to determine the result of the comparison.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.
*(since C++20)*

### Parameters

- **lhs, rhs** — tuples to compare

### Return value

1,8) `true` if `std::get<i>(lhs) == std::get<i>(rhs)` for all `i` in
   `[``​0​``,``sizeof...(Types)``)`, otherwise `false`. For two empty tuples
   returns `true`.

2) `!(lhs == rhs)`

3) `true` if the first non-equivalent element in `lhs` is less than the one in
   `rhs`, `false` if the first non-equivalent element in `rhs` is less than the
   one in `lhs` or there is no non-equivalent element. For two empty tuples,
   returns `false`.

4) `!(rhs < lhs)`

5) `rhs < lhs`

6) `!(lhs < rhs)`

7,9) The relation between the first pair of non-equivalent elements if there is
   any, `std::strong_ordering::equal` otherwise. For two empty tuples, returns
   `std::strong_ordering::equal`.

### Example

Because operator< is defined for tuples, containers of tuples can be sorted.

```cpp
#include <algorithm>
#include <iostream>
#include <tuple>
#include <vector>

int main()
{
    std::vector<std::tuple<int, std::string, float>> v
    {
        {2, "baz", -0.1},
        {2, "bar", 3.14},
        {1, "foo", 10.1},
        {2, "baz", -1.1},
    };
    std::sort(v.begin(), v.end());

    for (const auto& p: v)
        std::cout << "{ " << std::get<0>(p)
                  << ", " << std::get<1>(p)
                  << ", " << std::get<2>(p)
                  << " }\n";
}
```

Output:

```text
{ 1, foo, 10.1 }
{ 2, bar, 3.14 }
{ 2, baz, -1.1 }
{ 2, baz, -0.1 }
```

### See also

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values in
  the pair (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple/operator_cmp*
