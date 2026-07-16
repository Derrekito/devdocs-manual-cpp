# std::reverse_iterator<Iter>::base

```cpp
iterator_type base() const;  // (until C++17)
constexpr iterator_type base() const;  // (since C++17)
```

Returns the underlying base iterator. That is `std::reverse_iterator(it).base()
== it`.

The base iterator refers to the element that is next (from the
`std::reverse_iterator::iterator_type` perspective) to the element the
`reverse_iterator` is currently pointing to. That is `&*(rit.base() - 1) ==
&*rit`.

### Parameters

(none)

### Return value

The underlying iterator.

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v = {0, 1, 2, 3, 4, 5};

    using RevIt = std::reverse_iterator<std::vector<int>::iterator>;

    const auto it = v.begin() + 3;
    RevIt r_it{it};

    std::cout << "*it == " << *it << '\n'
              << "*r_it == " << *r_it << '\n'
              << "*r_it.base() == " << *r_it.base() << '\n'
              << "*(r_it.base()-1) == " << *(r_it.base() - 1) << '\n';

    RevIt r_end{v.begin()};
    RevIt r_begin{v.end()};

    for (auto it = r_end.base(); it != r_begin.base(); ++it)
        std::cout << *it << ' ';
    std::cout << '\n';

    for (auto it = r_begin; it != r_end; ++it)
        std::cout << *it << ' ';
    std::cout << '\n';
}
```

Output:

```text
*it == 3
*r_it == 2
*r_it.base() == 3
*(r_it.base()-1) == 2
0 1 2 3 4 5
5 4 3 2 1 0
```

### See also

- **operator*operator->** — accesses the pointed-to element (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/iterator/reverse_iterator/base*
