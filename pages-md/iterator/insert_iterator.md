# std::insert_iterator

```cpp
template< class Container >
class insert_iterator : public std::iterator<std::output_iterator_tag,
                                             void, void, void, void>  // (until C++17)
template< class Container >
class insert_iterator;  // (since C++17)
```

`std::insert_iterator` is a LegacyOutputIterator that inserts elements into a
container for which it was constructed, at the position pointed to by the
supplied iterator. The container's `insert()` member function is called whenever
the iterator (whether dereferenced or not) is assigned to. Incrementing the
`std::insert_iterator` is a no-op.

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

- **(constructor)** — constructs a new `insert_iterator` (public member
  function)
- **operator=** — inserts an object into the associated container (public member
  function)
- **operator*** — no-op (public member function)
- **operator++operator++(int)** — no-op (public member function)

### Member objects

- **`container` (protected member object)** — a pointer of type `Container*`
- **`iter` (protected member object)** — an iterator of type
  `Container::iterator`(until C++20) `ranges::iterator_t<Container>`(since
  C++20)

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <list>
#include <vector>

int main()
{
    std::vector<int> v{1, 2, 3, 4, 5};
    std::list<int> l{-1, -2, -3};
    std::copy(v.begin(), v.end(), // may be simplified with std::inserter
              std::insert_iterator<std::list<int>>(l, std::next(l.begin())));
    for (int n : l)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

Output:

```text
-1 1 2 3 4 5 -2 -3
```

### See also

- **inserter** — creates a `std::insert_iterator` of type inferred from the
  argument (function template)
- **back_insert_iterator** — iterator adaptor for insertion at the end of a
  container (class template)
- **front_insert_iterator** — iterator adaptor for insertion at the front of a
  container (class template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/insert_iterator*
