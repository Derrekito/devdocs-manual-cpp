# std::vector<T,Allocator>::pop_back

```cpp
void pop_back();  // (until C++20)
constexpr void pop_back();  // (since C++20)
```

Removes the last element of the container.

Calling `pop_back` on an empty container results in undefined behavior.

Iterators (including the `end()` iterator) and references to the last element
are invalidated.

### Parameters

(none)

### Return value

(none)

### Complexity

Constant.

### Exceptions

Throws nothing.

### Example

```cpp
#include <vector>
#include <iostream>

template<typename T>
void print(T const& xs)
{
    std::cout << "[ ";
    for (auto const& x : xs)
        std::cout << x << ' ';
    std::cout << "]\n";
}

int main()
{
    std::vector<int> numbers;

    print(numbers);

    numbers.push_back(5);
    numbers.push_back(3);
    numbers.push_back(4);

    print(numbers);

    numbers.pop_back();

    print(numbers);
}
```

Output:

```text
[ ]
[ 5 3 4 ]
[ 5 3 ]
```

### See also

- **push_back** — adds an element to the end (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/pop_back*
