# std::front_inserter

```cpp
template< class Container >
std::front_insert_iterator<Container> front_inserter( Container& c );  // (constexpr since C++20)
```

`front_inserter` is a convenience function template that constructs a
`std::front_insert_iterator` for the container `c` with the type deduced from
the type of the argument.

### Parameters

- **c** — container that supports a `push_front` operation

### Return value

A `std::front_insert_iterator` which can be used to add elements to the
beginning of the container `c`.

### Possible implementation

```cpp
template<class Container>
std::front_insert_iterator<Container> front_inserter( Container& c )
{
    return std::front_insert_iterator<Container>(c);
}
```

### Example

```cpp
#include <algorithm>
#include <deque>
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v{1, 2, 3, 4, 5};
    std::deque<int> d;
    std::copy(v.begin(), v.end(), std::front_inserter(d));
    for (int n : d)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

Output:

```text
5 4 3 2 1
```

### See also

- **front_insert_iterator** — iterator adaptor for insertion at the front of a
  container (class template)
- **back_inserter** — creates a `std::back_insert_iterator` of type inferred
  from the argument (function template)
- **inserter** — creates a `std::insert_iterator` of type inferred from the
  argument (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/front_inserter*
