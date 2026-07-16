# std::forward_list<T,Allocator>::erase_after

```cpp
iterator erase_after( const_iterator pos );  // (1) (since C++11)
iterator erase_after( const_iterator first, const_iterator last );  // (2) (since C++11)
```

Removes specified elements from the container.

1) Removes the element following `pos`.

2) Removes the elements following `first` until `last`.

### Parameters

- **pos** — iterator to the element preceding the element to remove
- **first, last** — range of elements to remove

### Return value

1) Iterator to the element following the erased one, or `end()` if no such
   element exists.

2) `last`

### Complexity

1) Constant.

2) Linear in distance between `first` and `last`.

### Example

```cpp
#include <forward_list>
#include <iostream>
#include <iterator>

int main()
{
    std::forward_list<int> l = {1, 2, 3, 4, 5, 6, 7, 8, 9};

//  l.erase(l.begin()); // Error: no function erase()

    l.erase_after(l.before_begin()); // Removes first element

    for (auto n : l)
        std::cout << n << ' ';
    std::cout << '\n';

    auto fi = std::next(l.begin());
    auto la = std::next(fi, 3);

    l.erase_after(fi, la);

    for (auto n : l)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

Output:

```text
2 3 4 5 6 7 8 9
2 3 6 7 8 9
```

### See also

- **clear** — clears the contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/forward_list/erase_after*
