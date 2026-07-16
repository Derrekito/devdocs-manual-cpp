# std::forward

`std::forward` only makes sense inside a template, on a forwarding
reference (a `T&&` parameter where `T` is a deduced, cv-unqualified
template parameter). Called as `std::forward<T>(t)`, it hands `t` on
to the next function with the same value category — lvalue or rvalue —
that it had when the caller passed it in. Unlike `std::move`, which
unconditionally casts to an rvalue, `forward` casts conditionally
based on `T`: that conditional cast is the entire reason it exists.

```cpp skip
template<class T>
void wrapper(T&& arg)
{
    foo(std::forward<T>(arg));   // forwards arg with its original value category
}
```

### What you provide

- **t** — the forwarding-reference parameter to pass on, and the
  explicit template argument `T` deduced from it by the enclosing
  template. Passing anything other than the `T` deduced for that
  parameter defeats the point.

### Guarantees and costs

- `constexpr` since C++14 (plain, non-`constexpr` `noexcept` before
  that); `noexcept` in every version since C++11.
- Constant-time: it compiles down to `static_cast<T&&>(t)` and nothing
  more.
- If a call to `wrapper()` passes an rvalue, `T` deduces to the
  unqualified type and `forward` produces an rvalue reference; if it
  passes a const lvalue, `T` deduces to `const U&` and `forward`
  produces a const lvalue reference; if it passes a non-const lvalue,
  `T` deduces to `U&` and `forward` produces a non-const lvalue
  reference. Each case reproduces exactly what the caller passed.
- Instantiating the rvalue-only overload with an lvalue reference type
  for `T` — i.e. attempting to forward an rvalue as an lvalue — is a
  compile-time error, not a silent conversion.

### Gotchas

- `std::forward<T>(t)` is only correct on a genuine forwarding
  reference. On an ordinary named `T&&` parameter that isn't deduced
  (e.g. a move constructor's `A&&`), use `std::move` instead — the
  parameter is always an lvalue inside the function body, and
  forwarding adds nothing there.
- Forwarding the result of a member call that itself depends on value
  category (an object with `&`/`&&`-qualified overloads of the same
  member) needs `std::forward<decltype(expr)>(expr)` at each step —
  dropping the intermediate `forward` silently collapses everything to
  an lvalue.
- Calling `std::forward<T>(t)` more than once on the same `t` inside a
  function is the same hazard as using a moved-from object twice: the
  first forward to an rvalue-accepting overload may leave `t` moved
  from.

### Example

```cpp
#include <iostream>
#include <memory>
#include <utility>

struct A
{
    A(int&& n) { std::cout << "rvalue overload, n=" << n << '\n'; }
    A(int& n)  { std::cout << "lvalue overload, n=" << n << '\n'; }
};

template<class T, class U>
std::unique_ptr<T> make_unique1(U&& u)
{
    return std::unique_ptr<T>(new T(std::forward<U>(u)));
}

int main()
{
    auto p1 = make_unique1<A>(2);   // rvalue
    int i = 1;
    auto p2 = make_unique1<A>(i);   // lvalue
}
```

```text
rvalue overload, n=2
lvalue overload, n=1
```

### Reference

```cpp skip
template< class T >
T&& forward( typename std::remove_reference<T>::type& t ) noexcept;  // (since C++11) (until C++14)
template< class T >
constexpr T&& forward( std::remove_reference_t<T>& t ) noexcept;  // (since C++14)
template< class T >
T&& forward( typename std::remove_reference<T>::type&& t ) noexcept;  // (since C++11) (until C++14)
template< class T >
constexpr T&& forward( std::remove_reference_t<T>&& t ) noexcept;  // (since C++14)
```

Return value: `static_cast<T&&>(t)`. Complexity: constant. The second
overload (rvalue reference parameter) exists to forward the result of
an expression — such as a call to an `&&`-qualified member function —
as the original value category of a forwarding-reference argument;
instantiating it with an lvalue reference type for `T` is ill-formed,
which is what makes forwarding an rvalue as an lvalue a compile error
rather than a silent bug.

### See also

- **move** (C++11) — unconditionally casts to an rvalue reference
- **move_if_noexcept** (C++11) — rvalue reference only if the move
  constructor is `noexcept`, else a copy-preserving lvalue

---
*Source: https://en.cppreference.com/w/cpp/utility/forward*
