# std::array<T,N>::at

```cpp
reference at( size_type pos );  // (1) (since C++11) (constexpr since C++17)
const_reference at( size_type pos ) const;  // (2) (since C++11) (constexpr since C++14)
```

Returns a reference to the element at specified location `pos`, with bounds
checking.

If `pos` is not within the range of the container, an exception of type
`std::out_of_range` is thrown.

### Parameters

- **pos** — position of the element to return

### Return value

Reference to the requested element.

### Exceptions

`std::out_of_range` if `pos >= size()`.

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <array>
#include <stdexcept>

#ifdef __GNUG__
[[gnu::noinline]]
#endif
unsigned int runtime_six() // Emulate runtime input
{
    return 6u;
}

int main()
{
    std::array<int, 6> data = {1, 2, 4, 5, 5, 6};

    // Set element 1
    data.at(1) = 88;

    // Read element 2
    std::cout << "Element at index 2 has value " << data.at(2) << '\n';

    std::cout << "data size = " << data.size() << '\n';

    try
    {
        // Set element 6, where the index is determined at runtime
        data.at(runtime_six()) = 666;
    }
    catch (std::out_of_range const& exc)
    {
        std::cout << exc.what() << '\n';
    }

    // Print final values
    std::cout << "data:";
    for (int elem : data)
        std::cout << ' ' << elem;
    std::cout << '\n';
}
```

Possible output:

```text
Element at index 2 has value 4
data size = 6
array::at: __n (which is 6) >= _Nm (which is 6)
data: 1 88 4 5 5 6
```

### See also

- **operator[]** — access specified element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/array/at*
