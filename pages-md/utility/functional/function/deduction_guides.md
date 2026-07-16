# deduction guides for `std::function`

```cpp
template< class R, class... ArgTypes >
function( R(*)(ArgTypes...) ) -> function<R(ArgTypes...)>;  // (1) (since C++17)
template< class F >
function( F ) -> function</*see below*/>;  // (2) (since C++17)
template< class F >
function( F ) -> function</*see below*/>;  // (3) (since C++23)
template< class F >
function( F ) -> function</*see below*/>;  // (4) (since C++23)
```

1) This deduction guide is provided for `std::function` to allow deduction from
   functions.

2) This overload participates in overload resolution only if `&F::operator()` is
   well-formed when treated as an unevaluated operand and
   `decltype(&F::operator())` is of the form `R(G::*)(A...)` (optionally
   cv-qualified, optionally noexcept, optionally lvalue reference qualified).
   The deduced type is `std::function<R(A...)>`.

3) This overload participates in overload resolution only if `&F::operator()` is
   well-formed when treated as an unevaluated operand and `F::operator()` is an
   explicit object parameter function whose type is of form `R(G, A...)` or
   `R(G, A...) noexcept`. The deduced type is `std::function<R(A...)>`.

4) This overload participates in overload resolution only if `&F::operator()` is
   well-formed when treated as an unevaluated operand and `F::operator()` is a
   static member function whose type is of form `R(A...)` or `R(A...) noexcept`.
   The deduced type is `std::function<R(A...)>`.

### Notes

These deduction guides do not allow deduction from a function with ellipsis
parameter, and the `...` in the types is always treated as a pack expansion.

The type deduced by these deduction guides may change in a later standard
revision (in particular, this might happen if `noexcept` support is added to
`std::function` in a later standard).

### Example

```cpp
#include <functional>
int func(double) { return 0; }
int main() {
  std::function f{func}; // guide #1 deduces function<int(double)>
  int i = 5;
  std::function g = [&](double) { return i; }; // guide #2 deduces function<int(double)>
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3238 | C++17 | behavior of (2) was unclear when `F::operator()` is
      &&-qualified | clarified to be excluded from overload resolution

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/function/deduction_guides*
