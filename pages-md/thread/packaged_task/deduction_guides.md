# deduction guides for `std::packaged_task`

```cpp
template< class R, class... Args >
packaged_task( R(*)(Args...) ) -> packaged_task<R(Args...)>;  // (1) (since C++17)
template< class F >
packaged_task( F ) -> packaged_task</*see below*/>;  // (2) (since C++17)
template< class F >
packaged_task( F ) -> packaged_task</*see below*/>;  // (3) (since C++23)
template< class F >
packaged_task( F ) -> packaged_task</*see below*/>;  // (4) (since C++23)
```

1) This deduction guide is provided for `std::packaged_task` to allow deduction
   from functions.

2) This overload participates in overload resolution only if `&F::operator()` is
   well-formed when treated as an unevaluated operand and
   `decltype(&F::operator())` is of the form `R(G::*)(A...)` (optionally
   cv-qualified, optionally noexcept, optionally lvalue reference qualified).
   The deduced type is `std::packaged_task<R(A...)>`.

3) This overload participates in overload resolution only if `&F::operator()` is
   well-formed when treated as an unevaluated operand and `F::operator()` is an
   explicit object parameter function whose type is of form `R(G, A...)` or
   `R(G, A...) noexcept`. The deduced type is `std::packaged_task<R(A...)>`.

4) This overload participates in overload resolution only if `&F::operator()` is
   well-formed when treated as an unevaluated operand and `F::operator()` is a
   static member function whose type is of form `R(A...)` or `R(A...) noexcept`.
   The deduced type is `std::packaged_task<R(A...)>`.

### Notes

These deduction guides do not allow deduction from a function with ellipsis
parameter, and the `...` in the types is always treated as a pack expansion.

### Example

```cpp
#include <future>

int func(double) { return 0; }

int main()
{
    std::packaged_task f{func}; // deduces packaged_task<int(double)>

    int i = 5;
    std::packaged_task g = [&](double) { return i; }; // => packaged_task<int(double)>
}
```

---
*Source: https://en.cppreference.com/w/cpp/thread/packaged_task/deduction_guides*
