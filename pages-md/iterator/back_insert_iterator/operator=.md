# std::back_insert_iterator<Container>::operator=

```cpp
back_insert_iterator<Container>&
    operator=( typename Container::const_reference value );  // (until C++11)
back_insert_iterator<Container>&
    operator=( const typename Container::value_type& value );  // (since C++11) (until C++20)
constexpr back_insert_iterator<Container>&
    operator=( const typename Container::value_type& value );  // (since C++20)
back_insert_iterator<Container>&
    operator=( typename Container::value_type&& value );  // (since C++11) (until C++20)
constexpr back_insert_iterator<Container>&
    operator=( typename Container::value_type&& value );  // (since C++20)
```

Inserts the given value `value` to the container.

1) Results in `container->push_back(value)`.

2) Results in `container->push_back(std::move(value))`.

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
    std::back_insert_iterator<std::deque<int>> it(q);

    for (int i = 0; i < 10; ++i)
        it = i; // calls q.push_back(i)

    for (auto& elem : q)
        std::cout << elem << ' ';
    std::cout << '\n';
}
```

Output:

```text
0 1 2 3 4 5 6 7 8 9
```

---
*Source: https://en.cppreference.com/w/cpp/iterator/back_insert_iterator/operator%3D*
