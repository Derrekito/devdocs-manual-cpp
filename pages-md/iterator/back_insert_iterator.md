# std::back_insert_iterator

```cpp
template< class Container >
class back_insert_iterator
    : public std::iterator<std::output_iterator_tag, void, void, void, void>  // (until C++17)
template< class Container >
class back_insert_iterator;  // (since C++17)
```

`std::back_insert_iterator` is a LegacyOutputIterator that appends elements to a
container for which it was constructed. The container's `push_back()` member
function is called whenever the iterator (whether dereferenced or not) is
assigned to. Incrementing the `std::back_insert_iterator` is a no-op.

### Member types

- **`iterator_category`** — std::output_iterator_tag
- **`value_type`** — void
- **`difference_type`** — void (until C++20) `std::ptrdiff_t` (since C++20)
- **void** — (until C++20)
- **`std::ptrdiff_t`** — (since C++20)
- **`pointer`** — void
- **`reference`** — void
- **`container_type`** — `Container`

Member types `iterator_category`, `value_type`, `difference_type`, `pointer` and
`reference` are required to be obtained by inheriting from
std::iterator<std::output_iterator_tag, void, void, void, void>.
*(until C++17)*

### Member functions

- **(constructor)** — constructs a new `back_insert_iterator` (public member
  function)
- **operator=** — inserts an object into the associated container (public member
  function)
- **operator*** — no-op (public member function)
- **operator++operator++(int)** — no-op (public member function)

### Member objects

- **`container` (protected)** — a pointer of type `Container*`

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v;

    std::generate_n(
        std::back_insert_iterator<std::vector<int>>(v),
            // Or use std::back_inserter helper
            // Or use std::back_insert_iterator(v) C++17's syntax
        10,
        [n = 0]() mutable { return ++n; }
    );

    for (int n : v)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

Output:

```text
1 2 3 4 5 6 7 8 9 10
```

### See also

- **back_inserter** — creates a `std::back_insert_iterator` of type inferred
  from the argument (function template)
- **front_insert_iterator** — iterator adaptor for insertion at the front of a
  container (class template)
- **insert_iterator** — iterator adaptor for insertion into a container (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/back_insert_iterator*
