# std::move_iterator<Iter>::base

```cpp
iterator_type base() const;  // (since C++11) (until C++17)
constexpr iterator_type base() const;  // (since C++17) (until C++20)
constexpr const iterator_type& base() const& noexcept;  // (since C++20)
constexpr iterator_type base() &&;  // (2) (since C++20)
```

Returns the underlying base iterator.

1) Copy constructs the return value from the underlying iterator.
*(until C++20)*

1) Returns a reference to the underlying iterator.
*(since C++20)*

2) Move constructs the return value from the underlying iterator.

### Parameters

(none)

### Return value

1) A copy of the underlying iterator.
*(until C++20)*

1) A reference to the underlying iterator.
*(since C++20)*

2) An iterator move constructed from the underlying iterator.

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v{0, 1, 2, 3, 4};
    std::move_iterator<std::vector<int>::reverse_iterator>
        m1{v.rbegin()},
        m2{v.rend()};

    std::copy(m1.base(), m2.base(), std::ostream_iterator<int>(std::cout, " "));
    std::cout << '\n';
}
```

Output:

```text
4 3 2 1 0
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

- **operator*operator-> (C++11)(C++11)(deprecated in C++20)** — accesses the
  pointed-to element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/iterator/move_iterator/base*
