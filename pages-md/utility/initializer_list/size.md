# std::initializer_list<T>::size

```cpp
size_type size() const noexcept;  // (since C++11) (until C++14)
constexpr size_type size() const noexcept;  // (since C++14)
```

Returns the number of elements in the initializer list, i.e.
`std::distance(begin(), end())`.

### Parameters

(none)

### Return value

the number of elements in the initializer list

### Complexity

Constant

### Example

```cpp
#include <initializer_list>
int main() { static_assert(std::initializer_list{1,2,3,4}.size() == 4); }
```

---
*Source: https://en.cppreference.com/w/cpp/utility/initializer_list/size*
