# std::end(std::initializer_list)

```cpp
template< class E >
const E* end( std::initializer_list<E> il ) noexcept;  // (since C++11) (until C++14)
template< class E >
constexpr const E* end( std::initializer_list<E> il ) noexcept;  // (since C++14)
```

The overload of `std::end` for `initializer_list` returns a pointer to one past
the last element of `il`.

### Parameters

- **il** — an `initializer_list`

### Return value

`il.end()`

### Example

```cpp
#include <iostream>
#include <iterator>
#include <algorithm>
#include <initializer_list>

int main()
{
    std::initializer_list il = {3, 1, 4, 1, 5, 9};

    std::reverse_copy(std::begin(il),
                      std::end(il),
                      std::ostream_iterator<int>(std::cout, " "));
}
```

Output:

```text
9 5 1 4 1 3
```

### See also

- **end** — returns a pointer to one past the last element (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/utility/initializer_list/end2*
