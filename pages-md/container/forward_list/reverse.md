# std::forward_list<T,Allocator>::reverse

```cpp
void reverse() noexcept;  // (since C++11)
```

Reverses the order of the elements in the container. No references or iterators
become invalidated.

### Parameters

(none)

### Return value

(none)

### Complexity

Linear in the size of the container.

### Example

```cpp
#include <iostream>
#include <forward_list>

std::ostream& operator<<(std::ostream& ostr, const std::forward_list<int>& list)
{
    for (auto& i : list)
        ostr << ' ' << i;
    return ostr;
}

int main()
{
    std::forward_list<int> list = {8, 7, 5, 9, 0, 1, 3, 2, 6, 4};
    std::cout << "initially: " << list << '\n';

    list.sort();
    std::cout << "ascending: " << list << '\n';

    list.reverse();
    std::cout << "descending:" << list << '\n';
}
```

Output:

```text
initially:  8 7 5 9 0 1 3 2 6 4
ascending:  0 1 2 3 4 5 6 7 8 9
descending: 9 8 7 6 5 4 3 2 1 0
```

### See also

- **sort** — sorts the elements (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/forward_list/reverse*
