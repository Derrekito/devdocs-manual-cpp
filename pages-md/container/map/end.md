# std::map<Key,T,Compare,Allocator>::end, std::map<Key,T,Compare,Allocator>::cend

```cpp
iterator end();  // (until C++11)
iterator end() noexcept;  // (since C++11)
const_iterator end() const;  // (until C++11)
const_iterator end() const noexcept;  // (since C++11)
const_iterator cend() const noexcept;  // (3) (since C++11)
```

Returns an iterator to the element following the last element of the `map`.

This element acts as a placeholder; attempting to access it results in undefined
behavior.

### Parameters

(none)

### Return value

Iterator to the element following the last element.

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <map>

int main()
{
    std::map<int, float> num_map;
    num_map[4] = 4.13;
    num_map[9] = 9.24;
    num_map[1] = 1.09;
    // Calls num_map.begin() and num_map.end()
    for (auto it = num_map.begin(); it != num_map.end(); ++it)
        std::cout << it->first << ", " << it->second << '\n';
}
```

Output:

```text
1, 1.09
4, 4.13
9, 9.24
```

#### Example using a custom comparison function

```cpp
#include <cmath>
#include <iostream>
#include <map>

struct Point { double x, y; };

// Compare the x-coordinates of two Point pointers.
struct PointCmp
{
    bool operator()(const Point* lhs, const Point* rhs) const
    {
        return lhs->x < rhs->x;
    }
};

int main()
{
    // Note that although the x-coordinates are out of order, the
    // map will be iterated through by increasing x-coordinates.
    Point points[3] = {{2, 0}, {1, 0}, {3, 0}};

    // mag is a map sending the address of node to its magnitude in the x-y plane.
    // Although the keys are pointers-to-Point, we want to order the map by the
    // x-coordinates of the points and NOT by the addresses of the Points. This
    // is done by using the PointCmp class's comparison method.
    std::map<Point*, double, PointCmp> mag(
        {{points, 2}, {points + 1, 1}, {points + 2, 3}}
    );

    // Change each y-coordinate from 0 to the magnitude.
    for (auto iter = mag.begin(); iter != mag.end(); ++iter)
    {
        auto cur = iter->first; // Pointer to Node
        cur->y = mag[cur]; // Could also have used cur->y = iter->second;
    }

    // Update and print the magnitude of each node.
    for (auto iter = mag.begin(); iter != mag.end(); ++iter)
    {
        auto cur = iter->first;
        mag[cur] = std::hypot(cur->x, cur->y);
        std::cout << "The magnitude of (" << cur->x << ", " << cur->y << ") is ";
        std::cout << iter->second << '\n';
    }

    // Repeat the above with the range-based for loop.
    for (auto i : mag)
    {
        auto cur = i.first;
        cur->y = i.second;
        mag[cur] = std::hypot(cur->x, cur->y);
        std::cout << "The magnitude of (" << cur->x << ", " << cur->y << ") is ";
        std::cout << mag[cur] << '\n';
        // Note that in contrast to std::cout << iter->second << '\n'; above,
        // std::cout << i.second << '\n'; will NOT print the updated magnitude.
        // If auto &i : mag was used instead, it will print the updated magnitude.
    }
}
```

Output:

```text
The magnitude of (1, 1) is 1.41421
The magnitude of (2, 2) is 2.82843
The magnitude of (3, 3) is 4.24264
The magnitude of (1, 1.41421) is 1.73205
The magnitude of (2, 2.82843) is 3.4641
The magnitude of (3, 4.24264) is 5.19615
```

### See also

- **begincbegin (C++11)** — returns an iterator to the beginning (public member
  function)
- **endcend (C++11)(C++14)** — returns an iterator to the end of a container or
  array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/map/end*
