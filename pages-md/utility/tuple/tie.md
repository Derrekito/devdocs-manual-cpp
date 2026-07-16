# std::tie

Builds a tuple of lvalue references to the variables you pass, so
assigning a tuple- or pair-valued expression into it **unpacks that
value into your existing variables**. Use `std::ignore` in any slot
you want to discard. If you're introducing brand-new variable names
rather than reusing ones you already have, structured bindings
(C++17) are usually the clearer tool — `tie` earns its keep for
reusing variables (e.g. across loop iterations) or for building a
lexicographic comparison out of existing members.

```cpp skip
std::tie(a, b) = f();                    // unpack into existing a, b
std::tie(std::ignore, ok) = m.insert(v); // discard the first slot
std::tie(a, b) < std::tie(c, d);         // lexicographic comparison
```

### What you provide

- **args** — zero or more lvalues to bind by reference, or
  `std::ignore` in any position whose value you don't want.

### Guarantees and costs

- `noexcept`; `constexpr` since C++14.
- Returns `std::tuple<Types&...>` — references into `args`, not
  copies. No allocation, no element construction.
- `std::tuple` has a converting assignment from `std::pair`, so
  `std::tie` also works to unpack a pair (e.g. the result of
  `map::insert`), not just another tuple.

### Gotchas

- The returned tuple holds *references* — it must not outlive the
  variables passed to `tie`, and reassigning through it (its usual
  purpose) writes straight through to them.
- `tie(a, b) = tie(a, b)` — self-tie-assignment involving overlapping
  variables — behaves per normal tuple assignment order and can
  surprise if you expect a simultaneous swap; use `std::swap` for
  swapping instead.
- For simply *creating* new named variables from a multi-value
  return, structured bindings (`auto [a, b] = f();`, C++17) are more
  direct than declaring variables first and `tie`-ing into them.

### Example

```cpp
#include <iostream>
#include <set>
#include <string>
#include <tuple>

struct S
{
    int n;
    std::string s;

    friend bool operator<(const S& lhs, const S& rhs)
    {
        // compare n first, then s, using existing members
        return std::tie(lhs.n, lhs.s) < std::tie(rhs.n, rhs.s);
    }
};

int main()
{
    std::set<S> items;
    S value{42, "Test"};

    std::set<S>::iterator it;
    bool inserted;
    std::tie(it, inserted) = items.insert(value);   // unpack the pair
    std::cout << "inserted: " << std::boolalpha << inserted << '\n';

    int x = 0, y = 0;
    auto pos = [](int w) { return std::tuple(w, 2 * w); };
    std::tie(x, y) = pos(3);   // reuse existing x, y
    std::cout << "x=" << x << " y=" << y << '\n';
}
```

```text
inserted: true
x=3 y=6
```

### Reference

```cpp skip
template< class... Types >
std::tuple<Types&...> tie( Types&... args ) noexcept;  // (since C++11) (until C++14)
template< class... Types >
constexpr std::tuple<Types&...> tie( Types&... args ) noexcept;  // (since C++14)
```

### See also

- **Structured binding** — declares new names bound to sub-objects or
  tuple elements of an initializer (C++17)
- **make_tuple** — creates a `tuple` from argument types (C++11)
- **forward_as_tuple** — creates a `tuple` of forwarding references (C++11)
- **tuple_cat** — concatenates tuples (C++11)
- **ignore** — placeholder that skips a slot when unpacking with `tie` (C++11)

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple/tie*
