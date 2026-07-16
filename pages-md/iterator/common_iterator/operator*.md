# std::common_iterator<I,S>::operator*,->

```cpp
constexpr decltype(auto) operator*();  // (1) (since C++20)
constexpr decltype(auto) operator*() const
    requires /*dereferenceable*/<const I>;  // (2) (since C++20)
constexpr auto operator->() const
    requires /* see description */;  // (3) (since C++20)
Helper types
class /*proxy*/ {
    std::iter_value_t<I> keep_;
    constexpr proxy(std::iter_reference_t<I>&& x)
        : keep_(std::move(x)) {}
public:
    constexpr const std::iter_value_t<I>* operator->() const noexcept {
        return std::addressof(keep_);
    }
};  // (4) (exposition only*)
```

Returns pointer or reference to the current element, or a proxy holding it.

The behavior is undefined if the underlying `std::variant` member object `var`
does not hold an object of type `I`, i.e. `std::holds_alternative<I>(var)` is
equal to `false`.

Let `it` denote the iterator of type `I` held by `var`, that is
`std::get<I>(var)`.

1,2) Returns the result of dereferencing `it`.
3) Returns a pointer or underlying iterator to the current element, or a proxy
holding it:

- Equivalent to `return it;`, if `I` is a pointer type or if the expression
  `it.operator->()` is well-formed.
- Otherwise, equivalent to `auto&& tmp = *it; return std::addressof(tmp);`, if
  `std::iter_reference_t<I>` is a reference type.
- Otherwise, equivalent to `return proxy(*it);`, where `proxy` is an exposition
  only class (4).

The expression in the `requires`-clause is equivalent to
   `std::indirectly_readable<const I> && ( requires(const I& i) {
   i.operator->(); } || std::is_reference_v<std::iter_reference_t<I>> ||
   std::constructible_from<std::iter_value_t<I>, std::iter_reference_t<I>> )`.

### Parameters

(none)

### Return value

1,2) Reference to the current element, or prvalue temporary. Equivalent to
   `*it`.

3) Pointer or iterator to the current element or proxy holding it as described
   above.

### Example

```cpp
#include <complex>
#include <initializer_list>
#include <iostream>
#include <iterator>

using std::complex_literals::operator""i;

int main()
{
    const auto il = {1i, 3.14 + 2i, 3i, 4i, 5i};

    using CI = std::common_iterator<
        std::counted_iterator<decltype(il)::iterator>,
        std::default_sentinel_t>;

    CI ci{std::counted_iterator{std::next(begin(il), 1), std::ssize(il) - 1}};

    std::cout << *ci << ' ' << ci->real() << '\n';
}
```

Output:

```text
(3.14,2) 3.14
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3574 | C++20 | `variant` was fully constexpr (P2231R1) but
      `common_iterator` was not | also made constexpr
  LWG 3595 | C++20 | functions of the proxy type lacked constexpr and noexcept |
      added
  LWG 3672 | C++20 | `operator->` might return by reference in usual cases |
      always returns by value

### See also

- **(constructor) (C++20)** — constructs a new iterator adaptor (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/iterator/common_iterator/operator**
