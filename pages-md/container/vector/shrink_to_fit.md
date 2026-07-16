# std::vector<T,Allocator>::shrink_to_fit

```cpp
void shrink_to_fit();  // (until C++20)
constexpr void shrink_to_fit();  // (since C++20)
```

Requests the removal of unused capacity.

It is a non-binding request to reduce `capacity()` to `size()`. It depends on
the implementation whether the request is fulfilled.

If reallocation occurs, all iterators (including the `end()` iterator) and all
references to the elements are invalidated. If no reallocation occurs, no
iterators or references are invalidated.

### Parameters

(none)

**Type requirements**

**-`T` must meet the requirements of MoveInsertable into `*this`. (since C++11)**

### Return value

(none)

### Complexity

At most linear in the size of the container.

### Exceptions
If an exception is thrown other than by the move constructor of a
non-CopyInsertable `T`, there are no effects.
*(since C++11)*

### Notes

In libstdc++, `shrink_to_fit()` is not available in C++98 mode.

### Example

```cpp
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v;
    std::cout << "Default-constructed capacity is " << v.capacity() << '\n';
    v.resize(100);
    std::cout << "Capacity of a 100-element vector is " << v.capacity() << '\n';
    v.resize(50);
    std::cout << "Capacity after resize(50) is " << v.capacity() << '\n';
    v.shrink_to_fit();
    std::cout << "Capacity after shrink_to_fit() is " << v.capacity() << '\n';
    v.clear();
    std::cout << "Capacity after clear() is " << v.capacity() << '\n';
    v.shrink_to_fit();
    std::cout << "Capacity after shrink_to_fit() is " << v.capacity() << '\n';
    for (int i = 1000; i < 1300; ++i)
        v.push_back(i);
    std::cout << "Capacity after adding 300 elements is " << v.capacity() << '\n';
    v.shrink_to_fit();
    std::cout << "Capacity after shrink_to_fit() is " << v.capacity() << '\n';
}
```

Possible output:

```text
Default-constructed capacity is 0
Capacity of a 100-element vector is 100
Capacity after resize(50) is 100
Capacity after shrink_to_fit() is 50
Capacity after clear() is 50
Capacity after shrink_to_fit() is 0
Capacity after adding 300 elements is 512
Capacity after shrink_to_fit() is 300
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 755 | C++98 | `std::vector` lacked explicit shrink-to-fit operations |
      provided
  LWG 2033 | C++11 | 1. `T` was not required to be MoveInsertable 2. the
      complexity requirement was missing | 1. required 2. added

### See also

- **size** — returns the number of elements (public member function)
- **capacity** — returns the number of elements that can be held in currently
  allocated storage (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/shrink_to_fit*
