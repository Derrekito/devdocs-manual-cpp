# std::any

`std::any` (C++17) is a type-erased container that holds a single
value of any copy-constructible type — or nothing at all. Unlike
`std::variant`, it isn't limited to a fixed set of alternatives named
in its type; unlike `std::optional`, the type it might hold isn't part
of its own type either. Getting the value back out requires knowing
(or checking) the type: `std::any_cast<T>` throws `std::bad_any_cast`
if the contained object isn't a `T`.

```cpp skip
std::any a = 1;                        // holds an int
a = 3.14;                              // now holds a double
a = std::string("hi");                 // now holds a std::string

a.has_value();                         // true unless empty
a.type();                              // std::type_info of the contained type

std::any_cast<double>(a);              // by value; throws bad_any_cast on mismatch
double* p = std::any_cast<double>(&a); // by pointer; nullptr on mismatch, no throw

a.reset();                             // back to empty
```

### Guarantees and costs

- Two `any` objects are equivalent if both are empty, or if both hold
  equivalent contained objects.
- Implementations are encouraged to avoid a dynamic allocation for
  small contained objects, but that optimization may only apply to
  types where `std::is_nothrow_move_constructible` is `true` — a
  throwing move constructor forces heap storage.
- `emplace` changes the contained object by constructing the new one
  in place, without going through a temporary.
- `any_cast<T>` on a value or reference throws `std::bad_any_cast` on
  a type mismatch; the pointer-returning overload returns `nullptr`
  instead of throwing.

### Gotchas

- `any_cast<T>` requires an *exact* type match (modulo cv/reference)
  — it does not attempt implicit conversions the way ordinary function
  calls do. Casting a `std::any` holding an `int` as `<double>` throws,
  even though `int` converts to `double` everywhere else.
- Checking `a.type()` against `typeid(T)` and then calling
  `any_cast<T>(&a)` avoids the throw path when a miss is expected to
  be common — prefer the pointer overload for probing.
- Holding any type at all comes at a cost: even the small-object
  optimization only kicks in for nothrow-move-constructible types, and
  there's no way to query which storage strategy an implementation
  chose.

### Example

```cpp
#include <any>
#include <iostream>

int main()
{
    std::any a = 1;
    std::cout << a.type().name() << ": " << std::any_cast<int>(a) << '\n';

    a = 3.14;
    std::cout << a.type().name() << ": " << std::any_cast<double>(a) << '\n';

    try
    {
        a = 1;
        std::cout << std::any_cast<float>(a) << '\n';
    }
    catch (const std::bad_any_cast& e)
    {
        std::cout << e.what() << '\n';
    }

    a.reset();
    std::cout << std::boolalpha << "has_value: " << a.has_value() << '\n';
}
```

```text
i: 1
d: 3.14
bad any_cast
has_value: false
```

### Reference

```cpp skip
class any;  // (since C++17)
```

An `any` object stores an instance of any type satisfying the
constructor requirements, or is empty; the stored instance is the
*contained object*.

**Member functions**: (constructor), `operator=`, (destructor).

- Modifiers
  - `emplace` — change the contained object, constructing in place
  - `reset` — destroy the contained object
  - `swap`
- Observers
  - `has_value`
  - `type` — `typeid` of the contained value

Non-member: `std::swap`, `any_cast` — type-safe access to the
contained object, `make_any` — constructs an `any` in place. Helper
class: `bad_any_cast`, thrown by the value-returning forms of
`any_cast` on a type mismatch.

### See also

- **function** (C++11) — wraps a callable of any copy-constructible
  type with a specified call signature
- **move_only_function** (C++23) — wraps a callable of any type with
  a specified call signature
- **variant** (C++17) — a type-safe discriminated union
- **optional** (C++17) — a wrapper that may or may not hold an object

---
*Source: https://en.cppreference.com/w/cpp/utility/any*
