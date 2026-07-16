# std::list<T,Allocator>::pop_back

```cpp
void pop_back();
```

Removes the last element of the container.

Calling `pop_back` on an empty container results in undefined behavior.

References and iterators to the erased element are invalidated.

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
#include <list>
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
    std::list<int> numbers;

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

- **pop_front** — removes the first element (public member function)
- **push_back** — adds an element to the end (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/list/pop_back*
