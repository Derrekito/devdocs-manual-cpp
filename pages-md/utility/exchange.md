# std::exchange

Replaces `obj`'s value with `new_value` and returns the *old* value —
a move-and-replace idiom in one call. Its main use is writing move
constructors and move assignment operators: `n{std::exchange(other.n,
0)}` moves `other.n` out while leaving a known, valid value (`0`)
behind, in a single expression instead of a separate "save old value,
then reset" pair.

```cpp skip
std::exchange(obj, new_value);  // (since C++14)
```

`constexpr` since C++20; `noexcept`-qualified since C++23 (see below).

### What you provide

- **obj** — the object to modify, taken by reference.
- **new_value** — the value to assign to `obj`. Its type `U` defaults
  to `T`, so a value, a braced-init-list, or a compatible object all
  work.

### Guarantees and costs

- Returns the value `obj` held *before* the assignment.
- `T` must be MoveConstructible, and objects of type `U` must be
  move-assignable to `T`.
- Since C++23, `noexcept` if `T`'s move constructor and the
  `T& = U` assignment are both non-throwing; before that, no
  exception guarantee is specified.

### Gotchas

- The old value is returned *by value* (a move out of `obj`, then
  `obj` is reassigned) — for expensive `T` this is exactly one move
  construction plus one move/copy assignment, not free.
- Because `new_value`'s type defaults to `T`, `std::exchange(v, {1, 2,
  3, 4})` and `std::exchange(fun, some_function)` both work without
  spelling out the type — useful, but means a typo in the intended
  type won't be caught by a mismatched second argument.
- Order of evaluation matters when chaining: `a = std::exchange(b, a
  + b)` for a Fibonacci-style update relies on `a + b` being computed
  with the *old* `a` and `b` before either is touched — the same
  building block used in `std::swap`-adjacent idioms.

### Example

```cpp
#include <iostream>
#include <utility>
#include <vector>

struct S
{
    int n;

    S(int v) : n(v) {}

    S(S&& other) noexcept : n{std::exchange(other.n, 0)} {}
};

int main()
{
    S a(42);
    S b(std::move(a));

    std::cout << "a.n = " << a.n << ", b.n = " << b.n << '\n';

    std::vector<int> v;
    std::exchange(v, {1, 2, 3, 4});
    for (int x : v)
        std::cout << x << ' ';
    std::cout << '\n';

    for (int x{0}, y{1}; x < 20; x = std::exchange(y, x + y))
        std::cout << x << ' ';
    std::cout << '\n';
}
```

```text
a.n = 0, b.n = 42
1 2 3 4 
0 1 1 2 3 5 8 13 
```

### Reference

```cpp skip
template< class T, class U = T >
T exchange( T& obj, U&& new_value );  // (since C++14) (until C++20)
template< class T, class U = T >
constexpr T exchange( T& obj, U&& new_value );  // (since C++20) (until C++23)
template< class T, class U = T >
constexpr T exchange( T& obj, U&& new_value ) noexcept(/* see below */);  // (since C++23)
```

`noexcept` since C++23:
`noexcept(std::is_nothrow_move_constructible_v<T> &&
std::is_nothrow_assignable_v<T&, U>)`.

Feature-test macro: `__cpp_lib_exchange_function` (`201304L`, C++14).

### See also

- **swap** — swaps the values of two objects
- **atomic_exchange** — atomically replaces the value of an atomic
  object, returning the old value (`atomic_exchange_explicit`, C++11)

---
*Source: https://en.cppreference.com/w/cpp/utility/exchange*
