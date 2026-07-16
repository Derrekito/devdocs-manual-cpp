# std::common_iterator<I,S>::operator++

```cpp
constexpr common_iterator& operator++();  // (1) (since C++20)
constexpr decltype(auto) operator++( int );  // (2) (since C++20)
Helper types
class /*postfix_proxy*/ {
    std::iter_value_t<I> keep_;
    constexpr postfix_proxy(std::iter_reference_t<I>&& x)
        : keep_(std::forward<std::iter_reference_t<I>>(x)) {}
public:
    constexpr const std::iter_value_t<I>& operator*() const noexcept {
        return keep_;
    }
};  // (3) (exposition only*)
```

Increments the underlying iterator.

The behavior is undefined if the underlying `std::variant` member object `var`
does not hold an object of type `I`, i.e. `std::holds_alternative<I>(var)` is
equal to `false`.

Let `it` denote the iterator of type `I` held by `var`, that is
`std::get<I>(var)`.

1) Pre-increments by one. Equivalent to `++it; return *this;`.
2) Post-increments by one:

- Equivalent to: `auto tmp = *this; ++*this; return tmp;`, if `I` models
  `forward_iterator`.
- Equivalent to: `return it++;`, if the variable definition `auto&& ref =
  *it++;` is well-formed, or either
- `std::indirectly_readable<I>` or
- `std::constructible_from<std::iter_value_t<I>, std::iter_reference_t<I>>` or
- `std::move_constructible<std::iter_value_t<I>>`

is `false`.

- Equivalent to: `postfix_proxy p(**this); ++*this; return p;` otherwise, where
  `postfix_proxy` is an exposition only helper type (3).

### Parameters

(none)

### Return value

1) `*this`

2) A copy of `*this` that was made before the change, or a result of
   post-increment of the underlying iterator, or a proxy keeping the value of
   the current element, as described above.

### Example

```cpp
#include <algorithm>
#include <initializer_list>
#include <iostream>
#include <iterator>

int main()
{
    const auto il = {1, 2, 3, 4, 5, 6};

    using CI = std::common_iterator<
                   std::counted_iterator<std::initializer_list<int>::iterator>,
                   std::default_sentinel_t
                   >;

    CI first{std::counted_iterator{std::begin(il), std::ssize(il) - 2}};

    for (; first != std::default_sentinel; ++first)
        std::cout << *first << ' ';
    std::cout << '\n';
}
```

Output:

```text
1 2 3 4
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2259R1 | C++20 | post increment might discard its result in more situations |
      a proxy class is used to keep the result
  LWG 3546 | C++20 | initialization of the proxy object was sometimes ill-formed
      | situation and definition adjusted
  LWG 3574 | C++20 | `variant` was fully constexpr (P2231R1) but
      `common_iterator` was not | also made constexpr
  LWG 3595 | C++20 | functions of the proxy type lacked constexpr and noexcept |
      added

### See also

- **operator-** — computes the distance between two iterator adaptors (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/common_iterator/operator_arith*
