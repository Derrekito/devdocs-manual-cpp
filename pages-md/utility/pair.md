# std::pair

`std::pair<T1, T2>` bundles two values of possibly different types
into a single object with named members `.first` and `.second`. It's
the element type of every associative container (`map`, `unordered_map`)
and a common lightweight "return two things" type — for more than two
values use `std::tuple`, of which `pair` is the two-element special
case.

```cpp skip
std::pair<T1, T2> p{a, b};
auto p = std::make_pair(a, b);       // deduces types (decays arrays/refs)
std::pair q{a, b};                   // CTAD, deduces exact types (C++17)
p.first, p.second                    // named access
auto [x, y] = p;                     // structured bindings (C++17)
```

### What you provide

- **T1, T2** — the element types.

### Member functions

| Member | What it does |
| --- | --- |
| `first`, `second` | the two stored values (public data members) |
| (constructor) | builds the pair from elements, another pair, or piecewise |
| `operator=` | assigns from another pair |
| `swap(other)` (C++11) | exchanges contents |

Non-members: `std::make_pair`, comparison operators (lexicographic:
`.first` compared first, `.second` breaks ties), `std::swap`,
`std::get<I>(p)`/`std::get<T>(p)` (index or type access, C++11).

### Guarantees and costs

- The destructor is trivial unless `T1` or `T2` is a (possibly
  array-of) class type with a non-trivial destructor — this triviality
  was unspecified before a defect report clarified it for C++98.
- Comparison is lexicographic and total: two pairs compare equal only
  when both elements do; ordering compares `.first`, then `.second` to
  break ties.
- No hidden cost beyond storing the two members — no allocation, no
  indirection.

### Gotchas

- `make_pair` decays its arguments the way a by-value function
  parameter would (arrays become pointers, references are dropped,
  `const` is stripped from prvalues) — brace-init a `std::pair<T1, T2>`
  directly, or use CTAD (`std::pair p{a, b}`, C++17), when you need the
  exact types.
- Lexicographic comparison always looks at `.second` to break ties on
  `.first`, even when that's not the ordering you meant — write a
  custom comparator when the fields aren't meant to be compared
  together.
- Prefer structured bindings (`auto [a, b] = p;`, C++17) to
  `.first`/`.second`, especially when iterating a map — it turns
  `it->first`/`it->second` noise into named variables.

### Example

```cpp
#include <iostream>
#include <string>
#include <utility>

int main()
{
    std::pair<int, std::string> p{1, "one"};
    std::cout << p.first << " = " << p.second << '\n';

    auto q = std::make_pair(2, std::string("two"));
    auto [n, word] = q;                          // structured bindings
    std::cout << n << " = " << word << '\n';

    std::cout << std::boolalpha << (p < q) << '\n';  // lexicographic
}
```

```text
1 = one
2 = two
true
```

### Reference

```cpp skip
template<
    class T1,
    class T2
> struct pair;
```

Formally: `first_type` names `T1`, `second_type` names `T2`. Helper
specializations `std::tuple_size<pair>` and `std::tuple_element<pair>`
(C++11) let `pair` participate in the tuple protocol (structured
bindings rely on this); `std::basic_common_reference` and
`std::common_type` specializations for `pair`/`pair`-like types, and a
`std::formatter` specialization shared with `tuple`, were added in
C++23. CTAD deduction guides were added in C++17. A defect report
(LWG 2796) specified pair's destructor triviality retroactively for
C++98.

### See also

- **tuple (C++11)** — a fixed-size collection of any number of values
- **tie (C++11)** — creates lvalue references or unpacks a tuple into variables

---
*Source: https://en.cppreference.com/w/cpp/utility/pair*
