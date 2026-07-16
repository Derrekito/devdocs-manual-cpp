# std::variant

`std::variant<Types...>` (C++17) is a type-safe union: an instance
holds a value of exactly one of its alternative types at a time, and
remembers which one — unlike a raw `union`, which knows nothing about
its own contents. Use it when a value can genuinely be one of several
unrelated types (a parsed token, a JSON value, an event) and you want
the compiler to check that every case is handled.

```cpp skip
std::variant<A, B, C> v;             // holds a default-constructed A
std::variant<A, B, C> v = b_value;    // holds a B
v.index()                             // which alternative (0-based)
std::holds_alternative<B>(v)          // true iff currently holds B
std::get<B>(v)                        // access; throws if not holding B
std::get_if<B>(&v)                    // access; null if not holding B
std::visit(callable, v)               // dispatch to the active alternative
v.valueless_by_exception()            // true only after a rare failure
```

Like a `union`, a `variant` never allocates: the active alternative's
representation lives directly inside the `variant` object. Reference,
array, and `void` alternatives are not allowed, and an empty
`variant<>` is ill-formed — use `variant<std::monostate>` when you
need a default-constructible placeholder alternative.

### What you provide

- **Types...** — the alternative types; each must be Destructible.
  The same type may appear more than once (disambiguate with
  `get<index>` instead of `get<T>` in that case).

### Member functions

| Member | What it does |
| --- | --- |
| `index()` | zero-based index of the currently held alternative |
| `valueless_by_exception()` | true if in the rare "no value" state |
| `emplace<T>(...)` / `emplace<I>(...)` | construct a new value in place |
| `swap(other)` | exchange contents |

Non-members: `std::visit` (applies a callable to whichever alternative
is active), `std::holds_alternative<T>`, `std::get<T>`/`std::get<I>`
(throwing access), `std::get_if<T>`/`std::get_if<I>` (non-throwing,
pointer-returning access), comparison operators, `std::swap`,
`std::hash<variant<Types...>>`.

Helper types: `std::monostate` (a default-constructible placeholder
alternative), `std::bad_variant_access` (thrown by `get` on the wrong
alternative), `std::variant_size`/`std::variant_size_v`,
`std::variant_alternative`/`std::variant_alternative_t`,
`std::variant_npos`.

### Guarantees and costs

- No dynamic allocation: the active alternative's storage lives inside
  the `variant` object, sized for the largest alternative.
- A default-constructed `variant` holds its **first** alternative,
  unless that alternative isn't default-constructible, in which case
  the whole `variant` isn't default-constructible either — put
  `std::monostate` first to opt back in.
- `valueless_by_exception()` is normally `false`; it becomes `true`
  only if an assignment's in-place construction of the new alternative
  throws partway through, leaving no valid alternative. This is rare
  in practice but must be handled by a generic `visit`.

### Gotchas

- `std::get<T>(v)` / `std::get<I>(v)` throws `std::bad_variant_access`
  when that alternative isn't the active one — use `get_if` when
  you're not sure and want a null pointer instead of a throw.
- A `visit` visitor must supply a callable for **every** alternative
  and all branches must return a common type — a missing case is a
  compile error, not a runtime one, which is the main safety benefit
  over a raw `union`. (The `overloaded{}` idiom for building that
  visitor from several lambdas is in this page's notes.)
- `index()` is 0-based and matches the order `Types...` were listed
  in, not any runtime-assigned identity.

### Example

```cpp
#include <cassert>
#include <iostream>
#include <string>
#include <variant>

int main()
{
    std::variant<int, float> v, w;
    v = 42;                          // v contains int
    int i = std::get<int>(v);
    assert(42 == i);
    w = std::get<0>(v);              // index access, same effect as get<int>

    try
    {
        std::get<float>(w);          // w contains int, not float: throws
    }
    catch (const std::bad_variant_access& ex)
    {
        std::cout << ex.what() << '\n';
    }

    std::variant<std::string, const void*> y("abc");
    // implicit conversion to const void* when passed a char const*
    assert(std::holds_alternative<const void*>(y));
    y = std::string("xyz");
    assert(std::holds_alternative<std::string>(y));
    std::cout << "ok\n";
}
```

```text
std::get: wrong index for variant
ok
```

### Reference

```cpp skip
template< class... Types >
class variant;  // (since C++17)
```

Formally: a variant is permitted to hold the same type more than once
and differently cv-qualified versions of the same type. Empty
`variant<>` and any alternative that is a reference, array, or `void`
are ill-formed. `valueless_by_exception()` is the state reachable only
via a throw during a type-changing assignment/emplace — it is
deliberately hard to reach.

Feature-test macro `__cpp_lib_variant`: `201606L` (C++17, the type
itself), `202102L` (DR applied to C++17, `visit` for classes derived
from `variant`), `202106L` (C++20 DR, fully `constexpr`), `202306L`
(C++26, member `visit`).

A defect report (LWG 2901) removed the `std::uses_allocator`
specialization for `variant`, which had been incorrectly provided —
`variant` does not support allocators.

### See also

- **optional (C++17)** — a wrapper that may or may not hold an object
- **monostate (C++17)** — placeholder for non-default-constructible alternatives
- **any (C++17)** — holds an instance of any CopyConstructible type

---
*Source: https://en.cppreference.com/w/cpp/utility/variant*
