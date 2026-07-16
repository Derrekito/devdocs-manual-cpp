# std::front_insert_iterator<Container>::operator=

```cpp
front_insert_iterator<Container>&
    operator=( typename Container::const_reference value );  // (until C++11)
front_insert_iterator<Container>&
    operator=( const typename Container::value_type& value );  // (since C++11) (until C++20)
constexpr front_insert_iterator<Container>&
    operator=( const typename Container::value_type& value );  // (since C++20)
front_insert_iterator<Container>&
    operator=( typename Container::value_type&& value );  // (since C++11) (until C++20)
constexpr front_insert_iterator<Container>&
    operator=( typename Container::value_type&& value );  // (since C++20)
```

Inserts the given value `value` to the container.

1) Results in `container->push_front(value)`.

2) Results in `container->push_front(std::move(value))`.

### Parameters

- **value** — the value to insert

### Return value

`*this`

### Example

```cpp
#include <deque>
#include <iostream>
#include <iterator>

int main()
{
    std::deque<int> q;
    std::front_insert_iterator<std::deque<int>> it(q);

    for (int i = 0; i < 10; ++i)
        it = i; // calls q.push_front(i)

    for (auto& elem : q)
        std::cout << elem << ' ';
    std::cout << '\n';
}
```

Output:

```text
9 8 7 6 5 4 3 2 1 0
```

---
*Source: https://en.cppreference.com/w/cpp/iterator/front_insert_iterator/operator%3D*
