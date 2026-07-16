# std::insert_iterator<Container>::operator=

```cpp
insert_iterator<Container>&
    operator=( typename Container::const_reference value );  // (until C++11)
insert_iterator<Container>&
    operator=( const typename Container::value_type& value );  // (since C++11) (until C++20)
constexpr insert_iterator<Container>&
    operator=( const typename Container::value_type& value );  // (since C++20)
insert_iterator<Container>&
    operator=( typename Container::value_type&& value );  // (since C++11) (until C++20)
constexpr insert_iterator<Container>&
    operator=( typename Container::value_type&& value );  // (since C++20)
```

Inserts the given value `value` to the container.

1) Results in `iter = container->insert(iter, value); ++iter;`.

2) Results in `iter = container->insert(iter, std::move(value)); ++iter;`.

### Parameters

- **value** — the value to insert

### Return value

`*this`

### Notes

This function exploits the signature compatibility between hinted insert for
associative containers (such as `std::set::insert`) and positional insert for
sequential containers (such as `std::vector::insert`).

### Example

```cpp
#include <deque>
#include <iostream>
#include <iterator>

int main()
{
    std::deque<int> q;
    std::insert_iterator<std::deque<int>> it(q, q.begin());

    for (int i = 0; i < 10; ++i)
        it = i; // inserts i

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
*Source: https://en.cppreference.com/w/cpp/iterator/insert_iterator/operator%3D*
