# std::move

`std::move` doesn't move anything — the name is a promise, not an
action. It's a **cast**: it produces an xvalue that makes its argument
eligible to be *moved from*, so the next constructor or assignment
that binds it prefers the move overload (steal internals) over the
copy overload (duplicate them). Nothing happens unless something
downstream actually reads that xvalue as an rvalue reference. C++11.

```cpp skip
std::move(t)                          // cast t to an rvalue reference
v.push_back(std::move(s));            // moves s into v instead of copying it
Buffer{std::move(tmp)};               // moves tmp into a member on construction
```

### What you provide

- **t** — the object you're done with and are handing off. Any object
  works; moving from a `const` object silently falls back to copying
  (you can't steal from `const`), which compiles but gains nothing.

### Guarantees and costs

- Equivalent to `static_cast<std::remove_reference_t<T>&&>(t)` —
  compiles to nothing by itself; the cost or benefit comes entirely
  from whichever overload picks up the resulting xvalue.
- `constexpr` since C++14 (plain `noexcept` before that, same
  behavior).
- Whether the receiving move constructor/assignment actually moves
  anything is up to that type — it's an option the overload has, not
  a requirement. A type with no move constructor just falls back to
  copying, silently.

### Gotchas

- After `std::move(x)`, `x` is left in a **valid but unspecified**
  state: still a live, destructible object whose invariants hold, but
  whose contents you must not rely on. Standard library types
  guarantee methods with no preconditions (`clear()`, assignment,
  `empty()`) remain safe to call; a precondition-bearing method like
  `back()` on an emptied container is still undefined behavior.
- Standard library functions that receive an xvalue argument may
  assume it's the *only* reference to that object — if you constructed
  the xvalue from an lvalue via `std::move`, no aliasing checks are
  performed. Self-move-assignment (`v = std::move(v);`) is the one
  exception the standard carves out: it's guaranteed to leave `v` in a
  valid, if unspecified, state.
- `return std::move(local);` on a local variable is almost always
  wrong: returning a local already triggers NRVO (compiler-elided, not
  guaranteed but done everywhere in practice) or an automatic move as
  of C++11 when elision doesn't apply. Wrapping it in `std::move`
  blocks the elision path and can force a move where the compiler
  could have skipped copying *and* moving. Just `return local;`.
- Move constructors/assignment operators must use `std::move`
  explicitly on their members — a named rvalue-reference parameter
  (like `arg` in `A(A&& arg)`) is itself an lvalue inside the function
  body. The one case that uses `std::forward` instead of `std::move`
  is a forwarding-reference parameter (`T&&` on a deduced template
  parameter).

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <string>
#include <utility>
#include <vector>

int main()
{
    std::string str = "Salut";
    std::vector<std::string> v;

    v.push_back(str);                 // copies str
    std::cout << "After copy, str is " << std::quoted(str) << '\n';

    v.push_back(std::move(str));      // moves str; str is now unspecified
    std::cout << "After move, str is " << std::quoted(str) << '\n';

    std::cout << "vector[0] = " << std::quoted(v[0]) << '\n';
}
```

```text
After copy, str is "Salut"
After move, str is ""
vector[0] = "Salut"
```

### Reference

```cpp skip
template< class T >
typename std::remove_reference<T>::type&& move( T&& t ) noexcept;  // (since C++11) (until C++14)
template< class T >
constexpr std::remove_reference_t<T>&& move( T&& t ) noexcept;  // (since C++14)
```

Return value: `static_cast<typename std::remove_reference<T>::type&&>(t)`.

Formally: names of rvalue-reference variables are themselves lvalues,
which is exactly why move constructors and move-assignment operators
must call `std::move` on each member they hand off — passing `arg`
(an `A&&` parameter) without `std::move` would bind to the copy
overload, not the move one.

### See also

- **forward (C++11)** — forwards a function argument, preserving value category
- **move_if_noexcept (C++11)** — rvalue reference only if the move
  constructor is `noexcept`, else a copy-preserving lvalue
- **move (C++11)** (`<algorithm>`) — moves a range of elements to a
  new location

---
*Source: https://en.cppreference.com/w/cpp/utility/move*
