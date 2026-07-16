# std::begin(std::initializer_list)

```cpp
template< class E >
const E* begin( std::initializer_list<E> il ) noexcept;  // (since C++11) (until C++14)
template< class E >
constexpr const E* begin( std::initializer_list<E> il ) noexcept;  // (since C++14)
```

The overload of `std::begin` for `initializer_list` returns a pointer to the
first element of `il`.

### Parameters

- **il** — an `initializer_list`

### Return value

`il.begin()`

### Example

```cpp
#include <iostream>
#include <iterator>
#include <algorithm>
#include <initializer_list>

int main()
{
    std::initializer_list il = {3, 1, 4, 1};

    std::copy(std::begin(il),
              std::end(il),
              std::ostream_iterator<int>(std::cout, "\n"));
}
```

Output:

```text
3
1
4
1
```

### See also

- **begin** — returns a pointer to the first element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/initializer_list/begin2*
