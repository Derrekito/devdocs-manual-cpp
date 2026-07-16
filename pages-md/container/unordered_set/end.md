# std::unordered_set<Key,Hash,KeyEqual,Allocator>::end, std::unordered_set<Key,Hash,KeyEqual,Allocator>::cend

```cpp
iterator end() noexcept;  // (1) (since C++11)
const_iterator end() const noexcept;  // (2) (since C++11)
const_iterator cend() const noexcept;  // (3) (since C++11)
```

Returns an iterator to the element following the last element of the
`unordered_set`.

This element acts as a placeholder; attempting to access it results in undefined
behavior.

### Parameters

(none)

### Return value

Iterator to the element following the last element.

### Complexity

Constant.

### Notes

Because both `iterator` and `const_iterator` are constant iterators (and may in
fact be the same type), it is not possible to mutate the elements of the
container through an iterator returned by any of these member functions.

### Example

```cpp
#include <iostream>
#include <unordered_set>

struct Point { double x, y; };

int main()
{
    Point pts[3] = {{1, 0}, {2, 0}, {3, 0}};

    // points is a set containing the addresses of points
    std::unordered_set<Point*> points = { pts, pts + 1, pts + 2 };

    // Change each y-coordinate of (i, 0) from 0 into i^2 and print the point
    for (auto iter = points.begin(); iter != points.end(); ++iter)
    {
        (*iter)->y = ((*iter)->x) * ((*iter)->x); // iter is a pointer-to-Point*
        std::cout << "(" << (*iter)->x << ", " << (*iter)->y << ") ";
    }
    std::cout << '\n';

    // Now using the range-based for loop, we increase each y-coordinate by 10
    for (Point* i : points)
    {
        i->y += 10;
        std::cout << "(" << i->x << ", " << i->y << ") ";
    }
}
```

Possible output:

```text
(3, 9) (1, 1) (2, 4)
(3, 19) (1, 11) (2, 14)
```

### See also

- **begincbegin** — returns an iterator to the beginning (public member
  function)
- **endcend (C++11)(C++14)** — returns an iterator to the end of a container or
  array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_set/end*
