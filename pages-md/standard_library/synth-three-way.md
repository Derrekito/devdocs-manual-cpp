# *synth-three-way*, *synth-three-way-result*

```cpp
constexpr auto synth-three-way = /* see below */;  // (1) (since C++20) (exposition only*)
template< class T, class U = T >
using synth-three-way-result =
    decltype(synth-three-way(std::declval<T&>(), std::declval<U&>()));  // (2) (since C++20) (exposition only*)
```

1) A function object whose `operator()` behaves as the synthesized three-way
   comparison function. Equivalent to: constexpr auto synth-three-way = []<class
   T, class U>(const T& t, const U& u) requires requires { { t < u } ->
   boolean-testable; { u < t } -> boolean-testable; } { if constexpr
   (std::three_way_comparable_with<T, U>) return t <=> u; else { if (t < u)
   return std::weak_ordering::less; if (u < t) return
   std::weak_ordering::greater; return std::weak_ordering::equivalent; } };

2) The return type of the `operator()` of (1) (`synth-three-way`).

### Parameters

- **t, u** — the values to be compared

### Return value

The compare result.

### See also

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values in
  the pair (function template)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values in
  the tuple (function template)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++11)(C++11)(removed in C++20)(C++11)(removed in C++20)(C++11)(removed in
  C++20)(C++11)(removed in C++20)(C++11)(removed in C++20)(C++20)** —
  lexicographically compares the values of two `arrays` (function template)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values of
  two `deques` (function template)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++11)(C++11)(removed in C++20)(C++11)(removed in C++20)(C++11)(removed in
  C++20)(C++11)(removed in C++20)(C++11)(removed in C++20)(C++20)** —
  lexicographically compares the values of two `forward_lists` (function
  template)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values of
  two `lists` (function template)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values of
  two `vectors` (function template)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values of
  two `maps` (function template)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values of
  two `multimaps` (function template)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values of
  two `sets` (function template)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values of
  two `multisets` (function template)

---
*Source: https://en.cppreference.com/w/cpp/standard_library/synth-three-way*
