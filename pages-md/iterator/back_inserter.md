# std::back_inserter

```cpp
template< class Container >
std::back_insert_iterator<Container> back_inserter( Container& c );  // (constexpr since C++20)
```

`back_inserter` is a convenience function template that constructs a
`std::back_insert_iterator` for the container `c` with the type deduced from the
type of the argument.

### Parameters

- **c** — container that supports a `push_back` operation

### Return value

A `std::back_insert_iterator` which can be used to add elements to the end of
the container `c`.

### Possible implementation

```cpp
template<class Container>
std::back_insert_iterator<Container> back_inserter(Container& c)
{
    return std::back_insert_iterator<Container>(c);
}
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    std::fill_n(std::back_inserter(v), 3, -1);
    for (int n : v)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

Output:

```text
1 2 3 4 5 6 7 8 9 10 -1 -1 -1
```

### See also

- **back_insert_iterator** — iterator adaptor for insertion at the end of a
  container (class template)
- **front_inserter** — creates a `std::front_insert_iterator` of type inferred
  from the argument (function template)
- **inserter** — creates a `std::insert_iterator` of type inferred from the
  argument (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/back_inserter*
