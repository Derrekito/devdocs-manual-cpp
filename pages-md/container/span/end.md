# std::span<T,Extent>::end, std::span<T,Extent>::cend

```cpp
constexpr iterator end() const noexcept;  // (1) (since C++20)
constexpr const_iterator cend() const noexcept;  // (2) (since C++23)
```

Returns an iterator to the element following the last element of the `span`.

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
#include <span>

void print(std::span<const int> array)
{
    std::cout << "array = ";
    for (auto it = array.begin(); it != array.end(); ++it)
        std::cout << *it << ' ';
    std::cout << '\n';
}

void set_first_element(std::span<int> sp, int new_value)
{
    if (!sp.empty())
    {
        std::cout << "old *begin = " << *sp.begin() << '\n';
        *sp.begin() = new_value;
        std::cout << "new *begin = " << *sp.begin() << '\n';
    }
}

int main()
{
    int array[]{1, 3, 4, 5};
    print(array);
    set_first_element(array, 2);
    print(array);
}
```

Output:

```text
array = 1 3 4 5
old *begin = 1
new *begin = 2
array = 2 3 4 5
```

### See also

- **begincbegin (C++23)** — returns an iterator to the beginning (public member
  function)
- **endcend (C++11)(C++14)** — returns an iterator to the end of a container or
  array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/span/end*
