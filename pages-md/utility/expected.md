# std::expected

`std::expected<T, E>` (C++23) holds a return value that is either a `T`
or an `E` — the error-handling alternative to exceptions and to
`std::optional` when you want to say *why* something failed, not just
*that* it failed. It always holds exactly one of the two (never
neither), and like `optional` the active value lives inline inside the
`expected` object: no dynamic allocation.

```cpp skip
std::expected<T, E> e{val};                    // holds a T
std::expected<T, E> e = std::unexpected(err);   // holds an E
if (e) ...                                      // true iff it holds a T
*e, e->member                                   // access T, UB if it holds E
e.value()                                       // access T, throws bad_expected_access<E> if E
e.error()                                       // access E, UB if it holds T
e.value_or(fallback)                            // T if present, else fallback
e.and_then(f) / e.transform(f)                  // chain on the T
e.or_else(f) / e.transform_error(f)             // chain on the E
```

### What you provide

- **T** — the expected value type. May be (possibly cv-qualified)
  `void`, or any type meeting the Destructible requirements; arrays
  and references are not allowed, and `T` cannot be `std::in_place_t`
  or `std::unexpect_t`.
- **E** — the error type. Must meet the Destructible requirements and
  be a valid argument to `std::unexpected` (no arrays, non-object
  types, or cv-qualified types).

### Member functions

| Member | What it does |
| --- | --- |
| `operator*`, `operator->` | access the value; **UB** if it holds an error |
| `value()` | access the value; throws `bad_expected_access<E>` if error |
| `error()` | access the error; **UB** if it holds a value |
| `value_or(v)` | value if present, else `v` |
| `has_value()`, `operator bool` | test which alternative is held |
| `and_then(f)` | apply `f` to the value, else keep `*this`'s error |
| `transform(f)` | map the value through `f`, else keep the error |
| `or_else(f)` | keep the value, else call `f` on the error |
| `transform_error(f)` | map the error through `f`, else keep the value |
| `emplace(args...)` | construct the value in place, replacing prior contents |
| `swap(other)` | exchange contents |

Non-members: `operator==`, `std::swap`. Helper types: `std::unexpected`
(wraps an `E` for construction/return), `std::bad_expected_access<E>`
(what `value()`/`error()` throw on the wrong access), `std::unexpect_t`
(in-place construction tag for the error alternative).

### Guarantees and costs

- No dynamic allocation: the active alternative occupies space inside
  the `expected` object itself, same as `optional`.
- `expected` is never valueless — it always holds a `T` or an `E`.
- Comparisons (`operator==`) and `swap` are defined between `expected`
  objects.

### Gotchas

- `*e` / `e->x` on an `expected` holding an error is **undefined
  behavior**; `.value()` instead throws `bad_expected_access<E>` —
  pick based on whether holding an error there is a bug (use `*`) or
  an expected outcome you handle (use `value()`).
- `.error()` is UB if `expected` currently holds a value — check
  `has_value()` (or `if (e)`) before reaching for `.error()`.
- A program is ill-formed if it instantiates `expected` with a
  reference type, a function type, or a specialization of
  `std::unexpected` as `T` or `E`.

### Example

```cpp c++23
#include <cmath>
#include <expected>
#include <iomanip>
#include <iostream>
#include <string_view>

enum class parse_error { invalid_input, overflow };

auto parse_number(std::string_view& str) -> std::expected<double, parse_error>
{
    const char* begin = str.data();
    char* end;
    double retval = std::strtod(begin, &end);

    if (begin == end)
        return std::unexpected(parse_error::invalid_input);
    if (std::isinf(retval))
        return std::unexpected(parse_error::overflow);

    str.remove_prefix(end - begin);
    return retval;
}

int main()
{
    for (std::string_view str : {"42", "meow", "inf"})
    {
        std::cout << std::quoted(str) << ": ";
        if (const auto num = parse_number(str); num.has_value())
            std::cout << "value " << *num << '\n';
        else if (num.error() == parse_error::invalid_input)
            std::cout << "invalid input\n";
        else
            std::cout << "overflow\n";
    }
}
```

```text
"42": value 42
"meow": invalid input
"inf": overflow
```

### Reference

```cpp skip
template< class T, class E >
class expected;  // (since C++23)
```

Formally: `T` must be (possibly cv-qualified) `void` or Destructible;
`E` must be Destructible and a valid `std::unexpected` argument.
`rebind<U>` names `expected<U, error_type>`. Types with the same
purpose are called `Result` in Rust and `Either` in Haskell.

Feature-test macro: `__cpp_lib_expected` — `202202L` (C++23, the class
template and helpers), `202211L` (C++23, monadic operations).

### See also

- **unexpected (C++23)** — wraps an error value for construction into `expected`
- **bad_expected_access (C++23)** — exception thrown by wrong-alternative access
- **variant (C++17)** — a type-safe discriminated union
- **optional (C++17)** — a wrapper that may or may not hold a value

---
*Source: https://en.cppreference.com/w/cpp/utility/expected*
