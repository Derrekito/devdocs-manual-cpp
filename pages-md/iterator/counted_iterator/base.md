# std::counted_iterator<I>::base

```cpp
constexpr const I& base() const& noexcept;  // (1) (since C++20)
constexpr I base() &&;  // (2) (since C++20)
```

Returns the underlying base iterator.

1) Returns a reference to the underlying iterator.

2) Move constructs the return value from the underlying iterator.

### Parameters

(none)

### Return value

1) A reference to the underlying iterator.

2) An iterator move constructed from the underlying iterator.

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <type_traits>
#include <vector>

int main()
{
    std::vector<int> v{0, 1, 2, 3, 4};

    std::reverse_iterator<std::vector<int>::iterator> reverse{v.rbegin()};

    std::counted_iterator counted{reverse, 3};

    static_assert(std::is_same<
        decltype(counted.base()),
        std::reverse_iterator<std::vector<int>::iterator> const&
    >{});

    std::cout << "Print with reverse_iterator: ";
    for (auto r = counted.base(); r != v.rend(); ++r)
        std::cout << *r << ' ';
    std::cout << '\n';

    std::cout << "Print with counted_iterator: ";
    for (; counted != std::default_sentinel; ++counted)
        std::cout << counted[0] << ' ';
    std::cout << '\n';
}
```

Output:

```text
Print with reverse_iterator: 4 3 2 1 0
Print with counted_iterator: 4 3 2
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3391 | C++20 | the const version of `base` returns a copy of the
      underlying iterator | returns a reference
  LWG 3593 | C++20 | the const version of `base` returns a reference but might
      not be noexcept | made noexcept

### See also

- **operator*operator-> (C++20)** — accesses the pointed-to element (public
  member function)
- **count (C++20)** — returns the distance to the end (public member function)

---
*Source: https://en.cppreference.com/w/cpp/iterator/counted_iterator/base*
