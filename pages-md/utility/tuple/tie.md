# std::tie

```cpp
template< class... Types >
std::tuple<Types&...> tie( Types&... args ) noexcept;  // (since C++11) (until C++14)
template< class... Types >
constexpr std::tuple<Types&...> tie( Types&... args ) noexcept;  // (since C++14)
```

Creates a tuple of lvalue references to its arguments or instances of
`std::ignore`.

### Parameters

- **args** — zero or more lvalue arguments to construct the tuple from.

### Return value

A `std::tuple` object containing lvalue references.

### Possible implementation

```cpp
template <typename... Args>
constexpr // since C++14
std::tuple<Args&...> tie(Args&... args) noexcept
{
    return {args...};
}
```

### Notes

`std::tie` may be used to unpack a `std::pair` because `std::tuple` has a
converting assignment from pairs:

```cpp
bool result;
std::tie(std::ignore, result) = set.insert(value);
```

### Example

1) `std::tie` can be used to introduce lexicographical comparison to a struct or
to unpack a tuple;
2) `std::tie` can work with structured bindings:

```cpp
#include <cassert>
#include <iostream>
#include <set>
#include <string>
#include <tuple>

struct S
{
    int n;
    std::string s;
    float d;

    friend bool operator<(const S& lhs, const S& rhs) noexcept
    {
        // compares lhs.n to rhs.n,
        // then lhs.s to rhs.s,
        // then lhs.d to rhs.d
        // in that order, first non-equal result is returned
        // or false if all elements are equal
        return std::tie(lhs.n, lhs.s, lhs.d) < std::tie(rhs.n, rhs.s, rhs.d);
    }
};

int main()
{
    // Lexicographical comparison demo:
    std::set<S> set_of_s;

    S value{42, "Test", 3.14};
    std::set<S>::iterator iter;
    bool is_inserted;

    // Unpack a pair:
    std::tie(iter, is_inserted) = set_of_s.insert(value);
    assert(is_inserted);

    // std::tie and structured bindings:
    auto position = [](int w) { return std::tuple(1 * w, 2 * w); };

    auto [x, y] = position(1);
    assert(x == 1 && y == 2);
    std::tie(x, y) = position(2); // reuse x, y with tie
    assert(x == 2 && y == 4);

    // Implicit conversions are permitted:
    std::tuple<char, short> coordinates(6, 9);
    std::tie(x, y) = coordinates;
    assert(x == 6 && y == 9);
}
```

### See also

- **Structured binding (C++17)** — binds the specified names to sub-objects or
  tuple elements of the initializer
- **make_tuple (C++11)** — creates a `tuple` object of the type defined by the
  argument types (function template)
- **forward_as_tuple (C++11)** — creates a `tuple` of forwarding references
  (function template)
- **tuple_cat (C++11)** — creates a `tuple` by concatenating any number of
  tuples (function template)
- **ignore (C++11)** — placeholder to skip an element when unpacking a `tuple`
  using **`tie`** (constant)

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple/tie*
