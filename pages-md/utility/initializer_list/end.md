# std::initializer_list<T>::end

```cpp
const T* end() const noexcept;  // (since C++11) (until C++14)
constexpr const T* end() const noexcept;  // (since C++14)
```

Returns a pointer to one past the last element in the initializer list, i.e.
`begin()`+`size()`.

If the initializer list is empty, the values of `begin()` and `end()` are
unspecified, but will be identical.

### Parameters

(none)

### Return value

a pointer to one past the last element in the initializer list

### Complexity

Constant

### Example

```cpp
#include <numeric>
#include <initializer_list>

int main()
{
    static constexpr auto l = {15, 14};
    static_assert(std::accumulate(l.begin(), l.end(), 13) == 42);
}
```

### See also

- **begin** — returns a pointer to the first element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/initializer_list/end*
