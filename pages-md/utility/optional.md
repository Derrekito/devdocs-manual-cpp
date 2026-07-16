# std::optional

`std::optional<T>` (C++17) wraps a value that may or may not be
present — a type-safe alternative to a sentinel value (`-1`, `nullptr`,
an out-parameter bool) for things like "the return of a lookup that can
fail." The value, when present, lives inline inside the `optional`
object itself; there is no heap allocation and no pointer semantics
despite `operator*`/`operator->` being defined.

```cpp skip
std::optional<T> o;                 // empty
std::optional<T> o{val};             // holds val
std::optional<T> o = std::nullopt;   // empty, explicit
if (o) ...                           // true iff it holds a value
*o, o->member                        // access, UB if empty
o.value()                            // access, throws if empty
o.value_or(fallback)                 // access with a fallback
o.and_then(f) / o.transform(f) / o.or_else(f)   // (since C++23)
```

There are no optional references and no `optional<optional<T>>`-style
tag types (`nullopt_t`, `in_place_t`) as the contained type — both are
ill-formed.

### What you provide

- **T** — the value type; must be Destructible. Arrays and reference
  types cannot be used.

### Member functions

| Member | What it does |
| --- | --- |
| `operator*`, `operator->` | access the value; **UB** if empty |
| `value()` | access the value; throws `bad_optional_access` if empty |
| `value_or(v)` | value if present, else `v` |
| `has_value()`, `operator bool` | test presence |
| `and_then(f)` (C++23) | apply `f` (returns `optional`), else empty |
| `transform(f)` (C++23) | apply `f` (returns plain value), wrap it |
| `or_else(f)` (C++23) | self if present, else call `f` for a fallback |
| `reset()` | clear to empty |
| `emplace(args...)` | construct the value in place, replacing prior one |
| `swap(other)` | exchange contents |

Non-members: `std::make_optional`, comparison operators, `std::swap`,
`std::hash<optional<T>>`.

### Guarantees and costs

- No dynamic allocation: the value occupies space inside the
  `optional` object, so a large `T` makes a large `optional`.
- Comparisons and `swap` are defined; an empty `optional` compares
  less than any `optional` that holds a value.
- Monadic operations (`and_then`, `transform`, `or_else`) are
  **C++23**; chain manually with `if` on C++17/20 (see the notes for
  this page).

### Gotchas

- `*o` / `o->x` on an empty optional is **undefined behavior**;
  `.value()` instead throws `std::bad_optional_access` — pick based on
  whether "empty" is a bug (use `*`) or an expected outcome you handle
  (use `value()`).
- `std::optional<bool>` has two different "false"s: empty, and holding
  `false`. Testing `if (o)` conflates them — check `has_value()`
  explicitly when that distinction matters.
- You cannot store a reference in an `optional` — instantiating
  `optional<T&>` is ill-formed; wrap in `std::reference_wrapper` if you
  need that.

### Example

```cpp
#include <iostream>
#include <optional>
#include <string>

// optional can be used as the return type of a factory that may fail
std::optional<std::string> create(bool b)
{
    if (b)
        return "Godzilla";
    return {};
}

int main()
{
    std::cout << "create(false) returned "
              << create(false).value_or("empty") << '\n';

    if (auto str = create(true))
        std::cout << "create(true) returned " << *str << '\n';
}
```

```text
create(false) returned empty
create(true) returned Godzilla
```

### Reference

```cpp skip
template< class T >
class optional;  // (since C++17)
```

Formally: an `optional<T>` *contains a value* once constructed or
assigned from a `T` (or a value-holding `optional`), and *does not
contain a value* when default-initialized, assigned from
`std::nullopt`, assigned from an empty `optional`, or after `reset()`.
`T` must be Destructible; `optional<void>`, `optional` of an array,
reference, `nullopt_t`, or `in_place_t` are all ill-formed. The
contextual conversion to `bool` reflects `has_value()`.

Feature-test macro: `__cpp_lib_optional` — `201606L` (C++17, the type
itself), `202106L` (C++20 DR, fully `constexpr`), `202110L` (C++23,
monadic operations).

### See also

- **variant (C++17)** — a type-safe discriminated union
- **any (C++17)** — holds an instance of any CopyConstructible type

---
*Source: https://en.cppreference.com/w/cpp/utility/optional*
