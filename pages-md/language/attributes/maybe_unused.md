# C++ attribute: maybe_unused (since C++17)

Suppresses warnings on unused entities.

### Syntax

```text
[[maybe_unused]]
```

### Explanation

This attribute can appear in the declaration of the following entities:

- class/struct/union: `struct [[maybe_unused]] S;`,
- typedef, including those declared by alias declaration: `[[maybe_unused]]
  typedef S* PS;`, `using PS [[maybe_unused]] = S*;`,
- variable, including static data member: `[[maybe_unused]] int x;`,
- non-static data member: `union U { [[maybe_unused]] int n; };`,
- function: `[[maybe_unused]] void f();`,
- enumeration: `enum [[maybe_unused]] E {};`,
- enumerator: `enum { A [[maybe_unused]], B [[maybe_unused]] = 42 };`,
- structured binding: `[[maybe_unused]] auto [a, b] = std::make_pair(42,
  0.23);`.

For entities declared `[[maybe_unused]]`, if the entities or their structured
bindings are unused, the warning on unused entities issued by the compiler is
suppressed.

### Example

```cpp
#include <cassert>

[[maybe_unused]] void f([[maybe_unused]] bool thing1,
                        [[maybe_unused]] bool thing2)
{
    [[maybe_unused]] bool b = thing1 && thing2;
    assert(b); // in release mode, assert is compiled out, and b is unused
               // no warning because it is declared [[maybe_unused]]
} // parameters thing1 and thing2 are not used, no warning

int main() {}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 2360 | C++17 | could not apply `[[maybe_unused]]` to structured bindings |
      allowed

### See also

**C documentation for `maybe_unused`**

---
*Source: https://en.cppreference.com/w/cpp/language/attributes/maybe_unused*
