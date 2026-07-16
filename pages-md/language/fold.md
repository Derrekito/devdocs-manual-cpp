# Fold expressions (since C++17)

Reduces (folds) a parameter pack over a binary operator.

### Syntax

```text
( pack op ... )    (1)
( ... op pack )    (2)
( pack op ... op init )    (3)
( init op ... op pack )    (4)
```

1) Unary right fold.

2) Unary left fold.

3) Binary right fold.

4) Binary left fold.

- **op** — any of the following 32 *binary* operators: `+` `-` `*` `/` `%` `^`
  `&` `|` `=` `<` `>` `<<` `>>` `+=` `-=` `*=` `/=` `%=` `^=` `&=` `|=` `<<=`
  `>>=` `==` `!=` `<=` `>=` `&&` `||` `,` `.*` `->*`. In a binary fold, both ops
  must be the same.
- **pack** — an expression that contains an unexpanded parameter pack and does
  not contain an operator with precedence lower than cast at the top level
  (formally, a cast-expression)
- **init** — an expression that does not contain an unexpanded parameter pack
  and does not contain an operator with precedence lower than cast at the top
  level (formally, a cast-expression)

Note that the opening and closing parentheses are a required part of the fold
expression.

### Explanation

The instantiation of a *fold expression* expands the expression `e` as follows:

1) Unary right fold `(E op ...)` becomes `(E1 op (... op (EN-1 op EN)))`

2) Unary left fold `(... op E)` becomes `(((E1 op E2) op ...) op EN)`

3) Binary right fold `(E op ... op I)` becomes `(E1 op (... op (EN−1 op (EN op
   I))))`

4) Binary left fold `(I op ... op E)` becomes `((((I op E1) op E2) op ...) op
   EN)`

(where `N` is the number of elements in the pack expansion)

For example,

```cpp
template<typename... Args>
bool all(Args... args) { return (... && args); }

bool b = all(true, true, true, false);
// within all(), the unary left fold expands as
//  return ((true && true) && true) && false;
// b is false
```

When a unary fold is used with a pack expansion of length zero, only the
following operators are allowed:

1) Logical AND (`&&`). The value for the empty pack is `true`.

2) Logical OR (`||`). The value for the empty pack is `false`.

3) The comma operator (`,`). The value for the empty pack is `void()`.

### Notes

If the expression used as init or as pack has an operator with precedence below
cast at the top level, it must be parenthesized:

```cpp
template<typename... Args>
int sum(Args&&... args)
{
//  return (args + ... + 1 * 2);   // Error: operator with precedence below cast
    return (args + ... + (1 * 2)); // OK
}
```

  Feature-test macro | Value | Std | Feature
  `__cpp_fold_expressions` | 201603L | (C++17) | Fold expressions

### Example

```cpp
#include <climits>
#include <concepts>
#include <cstdint>
#include <iostream>
#include <type_traits>
#include <utility>
#include <vector>

template<typename... Args>
void printer(Args&&... args)
{
    (std::cout << ... << args) << '\n';
}

template<typename T, typename... Args>
void push_back_vec(std::vector<T>& v, Args&&... args)
{
    static_assert((std::is_constructible_v<T, Args&&> && ...));
    (v.push_back(std::forward<Args>(args)), ...);
}

template<class T, std::size_t... dummy_pack>
constexpr T bswap_impl(T i, std::index_sequence<dummy_pack...>)
{
    T low_byte_mask = (unsigned char)-1;
    T ret{};
    ([&]
    {
        (void)dummy_pack;
        ret <<= CHAR_BIT;
        ret |= i & low_byte_mask;
        i >>= CHAR_BIT;
    }(), ...);
    return ret;
}

constexpr auto bswap(std::unsigned_integral auto i)
{
    return bswap_impl(i, std::make_index_sequence<sizeof(i)>{});
}

int main()
{
    printer(1, 2, 3, "abc");

    std::vector<int> v;
    push_back_vec(v, 6, 2, 45, 12);
    push_back_vec(v, 1, 2, 9);
    for (int i : v)
        std::cout << i << ' ';
    std::cout << '\n';

    static_assert(bswap<std::uint16_t>(0x1234u) == 0x3412u);
    static_assert(bswap<std::uint64_t>(0x0123456789abcdefull) == 0xefcdab8967452301ULL);
}
```

Output:

```text
123abc
6 2 45 12 1 2 9
```

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 2611 | C++17 | the expansion results of fold expressions were not enclosed
      with parentheses | enclosed with parentheses

---
*Source: https://en.cppreference.com/w/cpp/language/fold*
