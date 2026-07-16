# C++ named requirements: ReversibleContainer

A **ReversibleContainer** is a Container that has iterators that meet the
requirements of either LegacyBidirectionalIterator or
LegacyRandomAccessIterator. Such iterators allow a ReversibleContainer to be
iterated over in reverse.

### Requirements

- **`X`** — Container type
- **`T`** — Element type
- **`a`** — Objects of type `X`

#### Types

  Expression | Return type | Conditions
  `X::reverse_iterator` | Iterator type whose value type is `T` |
      `reverse_iterator<iterator>`
  `X::const_reverse_iterator` | Constant iterator type whose value type is `T` |
      `reverse_iterator<const_iterator>`

#### Member functions

  Expression | Return type | Conditions | Complexity
  `a.rbegin()` | `reverse_iterator`; `const_reverse_iterator` for constant `a` |
      `reverse_iterator(end())` | Constant
  `a.rend()` | `reverse_iterator`; `const_reverse_iterator` for constant `a` |
      `reverse_iterator(begin())` | Constant
  `a.crbegin()` | `const_reverse_iterator` | `const_cast<X const&>(a).rbegin()`
      | Constant
  `a.crend()` | `const_reverse_iterator` | `const_cast<X const&>(a).rend()` |
      Constant

### Standard library

- `std::array`
- `std::deque`
- `std::list`
- `std::vector`
- `std::map`
- `std::multimap`
- `std::set`
- `std::multiset`

### Example

The following example iterates over a `vector` (which has random-access
iterators) in reverse.

```cpp
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v = {3, 1, 4, 1, 5, 9};

    for (std::vector<int>::const_reverse_iterator i{v.crbegin()}; i != v.crend(); ++i)
        std::cout << *i << ' ';
    std::cout << '\n';
}
```

Output:

```text
9 5 1 4 1 3
```

---
*Source: https://en.cppreference.com/w/cpp/named_req/ReversibleContainer*
