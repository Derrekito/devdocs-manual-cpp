# std::initializer_list<T>::begin

```cpp
const T* begin() const noexcept;  // (since C++11) (until C++14)
constexpr const T* begin() const noexcept;  // (since C++14)
```

Returns a pointer to the first element in the initializer list.

If the initializer list is empty, the values of `begin()` and `end()` are
unspecified, but will be identical.

### Parameters

(none)

### Return value

a pointer to the first element in the initializer list

### Complexity

Constant

### Example

```cpp
#include <initializer_list>

int main()
{
    static constexpr auto il = {42, 24};
    static_assert(*il.begin() == 0x2A);
}
```

### See also

- **end** — returns a pointer to one past the last element (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/utility/initializer_list/begin*
