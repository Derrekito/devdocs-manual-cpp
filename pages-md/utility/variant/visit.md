# std::visit

Calls one function with whatever alternative a `std::variant`
currently holds, dispatching to the right handling automatically —
you write one visitor that knows how to handle every possible
alternative, and `visit` picks the right code path at runtime. The
visitor must be callable with *every* alternative type in *every*
variant passed, or the call fails to compile; that exhaustiveness is
the whole point. If a variant is left `valueless_by_exception` (only
possible after a throwing assignment), `visit` throws
`std::bad_variant_access` instead of calling the visitor.

```cpp skip
std::visit(vis, var);          // call vis with whatever var holds
std::visit(vis, var1, var2);   // one call per combination of alternatives
std::visit<R>(vis, var);       // explicit common return type   (since C++20)
```

### What you provide

- **vis** — a callable (lambda, function object, or a set of lambdas
  combined via the `overloaded` idiom) that accepts every alternative
  type from every variant passed in, in any combination.
- **vars** — one or more `std::variant`s to inspect.
- **R** — (C++20) an explicit return type every visitor result is
  converted to; without it, every code path in `vis` must produce the
  same type.

### Guarantees and costs

- Visiting a single variant dispatches in constant time regardless of
  how many alternatives it holds — implementations typically build a
  jump table or switch statement per `visit` instantiation, similar
  to how virtual calls dispatch.
- Visiting multiple variants at once has no complexity guarantee.
- Throws `std::bad_variant_access` if any variant argument is
  `valueless_by_exception()`.

### Gotchas

- The visitor must handle *every* alternative, in *every* variant, in
  *every* combination — a generic lambda using `if constexpr` chains
  needs a trailing `else` with `static_assert(false, ...)` (via a
  dependent trait like `always_false_v<T>`) so a genuinely missed
  type fails to compile instead of silently doing nothing.
- Combining several single-type lambdas into one visitor is the
  `overloaded` pattern: a struct that inherits every lambda's
  `operator()` via `using Ts::operator()...`, with a deduction guide
  (unneeded since C++20).
- Watch implicit conversions with non-generic overloads: a lambda
  taking `double` will also silently bind `int` or `long`
  alternatives unless a matching overload intercepts them first.

### Example

```cpp c++20
#include <iostream>
#include <string>
#include <variant>

using var_t = std::variant<int, double, std::string>;

// combine several single-type lambdas into one visitor
template<class... Ts>
struct overloaded : Ts... { using Ts::operator()...; };
template<class... Ts>
overloaded(Ts...) -> overloaded<Ts...>;

int main()
{
    var_t v = 15;

    // side-effecting visitor
    std::visit([](auto&& arg) { std::cout << arg << ' '; }, v);

    // value-returning visitor: double whatever the variant holds
    var_t doubled = std::visit([](auto&& arg) -> var_t { return arg + arg; }, v);
    std::visit([](auto&& arg) { std::cout << arg << ' '; }, doubled);

    // type-matching visitor via overloaded{}
    v = "hi";
    std::visit(overloaded{
        [](int i) { std::cout << "int " << i; },
        [](double d) { std::cout << "double " << d; },
        [](const std::string& s) { std::cout << "string " << s; }
    }, v);
    std::cout << '\n';
}
```

```text
15 30 string hi
```

### Reference

```cpp skip
template< class Visitor, class... Variants >
constexpr /* see below */ visit( Visitor&& vis, Variants&&... vars );  // (1) (since C++17)
template< class R, class Visitor, class... Variants >
constexpr R visit( Visitor&& vis, Variants&&... vars );  // (2) (since C++20)
```

Formally, `visit` invokes `vis` with the alternatives currently held
by each variant (obtained via `std::get`, using each variant's
`index()`); overload (2) converts the result to `R` (or discards it
if `R` is `void`). Both overloads participate in overload resolution
only if that invocation is well-formed for every combination of
alternatives and, for overload (1), every combination yields the same
result type and value category — otherwise the program is ill-formed.
Also accepts values of classes publicly and unambiguously derived from
`std::variant`.

### See also

- **variant** — the class template `visit` operates on
- **holds_alternative** — checks the currently active alternative
- **get** — accesses a specific alternative directly

---
*Source: https://en.cppreference.com/w/cpp/utility/variant/visit*
