# std::make_move_iterator

```cpp
template< class Iter >
std::move_iterator<Iter> make_move_iterator( Iter i );  // (since C++11) (until C++17)
template< class Iter >
constexpr std::move_iterator<Iter> make_move_iterator( Iter i );  // (since C++17)
```

`make_move_iterator` is a convenience function template that constructs a
`std::move_iterator` for the given iterator `i` with the type deduced from the
type of the argument.

### Parameters

- **i** — input iterator to be converted to move iterator

### Return value

A `std::move_iterator` which can be used to move from the elements accessed
through `i`.

### Possible implementation

```cpp
template<class Iter>
constexpr std::move_iterator<Iter> make_move_iterator(Iter i)
{
    return std::move_iterator<Iter>(std::move(i));
}
```

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <iterator>
#include <list>
#include <string>
#include <vector>

auto print = [](auto const rem, auto const& seq)
{
    for (std::cout << rem; auto const& str : seq)
        std::cout << std::quoted(str) << ' ';
    std::cout << '\n';
};

int main()
{
    std::list<std::string> s{"one", "two", "three"};

    std::vector<std::string> v1(s.begin(), s.end()); // copy

    std::vector<std::string> v2(std::make_move_iterator(s.begin()),
                                std::make_move_iterator(s.end())); // move

    print("v1 now holds: ", v1);
    print("v2 now holds: ", v2);
    print("original list now holds: ", s);
}
```

Possible output:

```text
v1 now holds: "one" "two" "three"
v2 now holds: "one" "two" "three"
original list now holds: "" "" ""
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2061 | C++11 | `make_move_iterator` did not convert array arguments to
      pointers | made to convert

### See also

- **move_iterator (C++11)** — iterator adaptor which dereferences to an rvalue
  reference (class template)
- **move (C++11)** — obtains an rvalue reference (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/make_move_iterator*
