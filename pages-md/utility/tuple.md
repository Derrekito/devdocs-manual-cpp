# std::tuple

`std::tuple<Types...>` (C++11) is a fixed-size collection of
heterogeneous values — a generalization of `std::pair` to any number
of elements. Reach for it when you need to bundle a handful of
unrelated values (often as a function's return type) without writing a
named struct; for anything you'll access by more than a positional
index in several places, a named struct documents intent better.

```cpp skip
std::tuple<A, B, C> t{a, b, c};
auto t = std::make_tuple(a, b, c);         // deduces element types
auto [x, y, z] = t;                        // structured bindings (C++17)
std::get<0>(t)                             // access by index (compile-time)
std::get<B>(t)                             // access by unique type
std::tie(x, y, z) = t;                     // unpack into existing variables
```

### What you provide

- **Types...** — the element types, in order. An empty tuple
  (`tuple<>`) is supported.

### Member functions

| Member | What it does |
| --- | --- |
| (constructor) | builds the tuple from elements or another tuple |
| `operator=` | assigns from another tuple |
| `swap(other)` | exchanges contents element-wise |

Non-members: `std::make_tuple` (deduces element types, decaying
arrays/references like a by-value parameter), `std::tie` (builds a
tuple of lvalue references, used to unpack or for field-by-field
comparison), `std::forward_as_tuple` (tuple of forwarding references),
`std::tuple_cat` (concatenates tuples), `std::get<I>`/`std::get<T>`,
comparison operators, `std::swap`.

Helpers: `std::tuple_size`/`std::tuple_size_v`,
`std::tuple_element`/`std::tuple_element_t`, `std::ignore` (a
write-only placeholder for `std::tie` to skip a field).

### Guarantees and costs

- The destructor is trivial when every element's destructor is
  trivial (this triviality is specified as of a defect report against
  C++11; it was previously unspecified).
- A tuple's size and element types are fixed at compile time — there
  is no way to filter or resize a tuple based on a runtime condition,
  only based on compile-time information about the types involved.
- `std::get<I>` and `std::tuple_element` resolve at compile time:
  `I` must be a constant expression, never a runtime index.

### Gotchas

- Before a defect report against C++11 (N4387), a function could not
  return a braced list for a tuple return type via copy-list-init;
  modern compilers all support `return {a, b, c};` for a `tuple`
  return type, but `std::make_tuple(...)` or an explicit `tuple{...}`
  always work regardless.
- `std::get<T>(t)` by type only compiles if `T` appears exactly once
  in the tuple; with a repeated type, use `std::get<I>` instead.
- Prefer structured bindings (C++17) over `std::tie`/`std::get` when
  just reading values — `std::tie` still earns its keep for unpacking
  into pre-existing variables or for lexicographic comparison of
  several fields at once.
- A tuple with many positional fields is a readability smell in
  interfaces others call — a named struct makes the meaning of each
  slot visible at the call site.

### Example

```cpp
#include <iostream>
#include <stdexcept>
#include <string>
#include <tuple>

std::tuple<double, char, std::string> get_student(int id)
{
    switch (id)
    {
        case 0: return {3.8, 'A', "Lisa Simpson"};
        case 1: return {2.9, 'C', "Milhouse Van Houten"};
    }
    throw std::invalid_argument("id");
}

int main()
{
    const auto student0 = get_student(0);
    std::cout << "ID: 0, "
              << "GPA: " << std::get<0>(student0) << ", "
              << "grade: " << std::get<1>(student0) << ", "
              << "name: " << std::get<2>(student0) << '\n';

    // C++17 structured binding:
    const auto [gpa1, grade1, name1] = get_student(1);
    std::cout << "ID: 1, "
              << "GPA: " << gpa1 << ", "
              << "grade: " << grade1 << ", "
              << "name: " << name1 << '\n';
}
```

```text
ID: 0, GPA: 3.8, grade: A, name: Lisa Simpson
ID: 1, GPA: 2.9, grade: C, name: Milhouse Van Houten
```

### Reference

```cpp skip
template< class... Types >
class tuple;  // (since C++11)
```

Formally: `Types...` must each meet the Destructible requirements
(array types and non-object types are not allowed). The "shape" of a
tuple — its size, element types, and their order — is part of its
type, so operations that would change the shape based on a runtime
value are not expressible; only compile-time-conditional operations
(e.g. filtering by type) are possible. `tuple-like`/`pair-like`
(C++23) is the exposition-only concept describing any type that
implements the tuple protocol (`get`, `tuple_element`, `tuple_size`),
used to extend `common_reference`, `common_type`, and `formatter`
support to `tuple`-shaped types generally.

### See also

- **pair** — a fixed two-element tuple, with named `.first`/`.second` access

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple*
