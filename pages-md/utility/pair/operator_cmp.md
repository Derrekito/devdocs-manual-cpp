# operator==,!=,<,<=,>,>=,<=>(std::pair)

```cpp
template< class T1, class T2, class U1, class U2 >
bool operator==( const std::pair<T1, T2>& lhs, const std::pair<U1, U2>& rhs );  // (until C++14)
template< class T1, class T2, class U1, class U2 >
constexpr bool operator==( const std::pair<T1, T2>& lhs,
                           const std::pair<U1, U2>& rhs );  // (since C++14)
template< class T1, class T2, class U1, class U2 >
bool operator!=( const std::pair<T1, T2>& lhs, const std::pair<U1, U2>& rhs );  // (until C++14)
template< class T1, class T2, class U1, class U2 >
constexpr bool operator!=( const std::pair<T1, T2>& lhs,
                           const std::pair<U1, U2>& rhs );  // (since C++14) (until C++20)
template< class T1, class T2, class U1, class U2 >
bool operator<( const std::pair<T1, T2>& lhs, const std::pair<U1, U2>& rhs );  // (until C++14)
template< class T1, class T2, class U1, class U2 >
constexpr bool operator<( const std::pair<T1, T2>& lhs,
                          const std::pair<U1, U2>& rhs );  // (since C++14) (until C++20)
template< class T1, class T2, class U1, class U2 >
bool operator<=( const std::pair<T1, T2>& lhs, const std::pair<U1, U2>& rhs );  // (until C++14)
template< class T1, class T2, class U1, class U2 >
constexpr bool operator<=( const std::pair<T1, T2>& lhs,
                           const std::pair<U1, U2>& rhs );  // (since C++14) (until C++20)
template< class T1, class T2, class U1, class U2 >
bool operator>( const std::pair<T1, T2>& lhs, const std::pair<U1, U2>& rhs );  // (until C++14)
template< class T1, class T2, class U1, class U2 >
constexpr bool operator>( const std::pair<T1, T2>& lhs,
                          const std::pair<U1, U2>& rhs );  // (since C++14) (until C++20)
template< class T1, class T2, class U1, class U2 >
bool operator>=( const std::pair<T1, T2>& lhs, const std::pair<U1, U2>& rhs );  // (until C++14)
template< class T1, class T2, class U1, class U2 >
constexpr bool operator>=( const std::pair<T1, T2>& lhs,
                           const std::pair<U1, U2>& rhs );  // (since C++14) (until C++20)
template< class T1, class T2, class U1, class U2 >
constexpr std::common_comparison_category_t<synth-three-way-result<T1, U1>,
                                            synth-three-way-result<T2, U2>>
    operator<=>( const std::pair<T1, T2>& lhs, const std::pair<U1, U2>& rhs );  // (7) (since C++20)
```

1,2) Tests if both elements of `lhs` and `rhs` are equal, that is, compares
   `lhs.first` with `rhs.first` and `lhs.second` with `rhs.second`.

3-6) Compares `lhs` and `rhs` lexicographically by `operator<`, that is,
   compares the first elements and only if they are equivalent, compares the
   second elements.

7) Compares `lhs` and `rhs` lexicographically by `synth-three-way`, that is,
   compares the first elements and only if they are equivalent, compares the
   second elements. `synth-three-way-result` is the return type of
   `synth-three-way`.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.
*(since C++20)*

### Parameters

- **lhs, rhs** — pairs to compare

### Return value

1) `true` if both `lhs.first == rhs.first` and `lhs.second == rhs.second`,
   otherwise `false`.

2) `!(lhs == rhs)`

3) If `lhs.first < rhs.first`, returns `true`. Otherwise, if `rhs.first <
   lhs.first`, returns `false`. Otherwise, if `lhs.second < rhs.second`, returns
   `true`. Otherwise, returns `false`.

4) `!(rhs < lhs)`

5) `rhs < lhs`

6) `!(lhs < rhs)`

7) `synth-three-way(lhs.first, rhs.first)` if it is not equal to `​0​`,
   otherwise `synth-three-way(lhs.second, rhs.second)`.

### Example

Because operator< is defined for pairs, containers of pairs can be sorted.

```cpp
#include <algorithm>
#include <iomanip>
#include <iostream>
#include <string>
#include <utility>
#include <vector>

int main()
{
    std::vector<std::pair<int, std::string>> v = {{2, "baz"}, {2, "bar"}, {1, "foo"}};
    std::sort(v.begin(), v.end());

    for (auto p : v)
        std::cout << '{' << p.first << ", " << std::quoted(p.second) << "}\n";
}
```

Output:

```text
{1, "foo"}
{2, "bar"}
{2, "baz"}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 296 | C++98 | the descriptions of operators other than `==` and `<` were
      missing | added
  LWG 3865 | C++98 | comparison operators only accepted `pair`s of the same type
      | accept `pair`s of different types

### See also

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values in
  the tuple (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/pair/operator_cmp*
