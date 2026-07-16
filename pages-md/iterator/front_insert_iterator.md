# std::front_insert_iterator

```cpp
template< class Container >
class front_insert_iterator
    : public std::iterator<std::output_iterator_tag, void, void, void, void>  // (until C++17)
template< class Container >
class front_insert_iterator;  // (since C++17)
```

`std::front_insert_iterator` is a LegacyOutputIterator that prepends elements to
a container for which it was constructed. The container's `push_front()` member
function is called whenever the iterator (whether dereferenced or not) is
assigned to. Incrementing the `std::front_insert_iterator` is a no-op.

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

- **(constructor)** — constructs a new `front_insert_iterator` (public member
  function)
- **operator=** — inserts an object into the associated container (public member
  function)
- **operator*** — no-op (public member function)
- **operator++operator++(int)** — no-op (public member function)

### Member objects

- **`container` (protected)** — a pointer of type Container*

### Example

```cpp
#include <algorithm>
#include <deque>
#include <iostream>
#include <iterator>
#include <vector>

namespace stb
{
    void println(auto, auto const& d)
    {
        std::ranges::copy(d, std::ostream_iterator<int>(std::cout, " "));
        std::cout << '\n';
    }
}

int main()
{
    std::vector<int> v{1, 2, 3, 4, 5};
    std::deque<int> d;

    std::copy(v.begin(), v.end(),
              std::front_insert_iterator<std::deque<int>>(d));
              // or std::front_inserter(d)

    stb::println("{}", d);
}
```

Output:

```text
5 4 3 2 1
```

### See also

- **front_inserter** — creates a `std::front_insert_iterator` of type inferred
  from the argument (function template)
- **back_insert_iterator** — iterator adaptor for insertion at the end of a
  container (class template)
- **insert_iterator** — iterator adaptor for insertion into a container (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/front_insert_iterator*
