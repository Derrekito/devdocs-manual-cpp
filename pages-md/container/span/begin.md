# std::span<T,Extent>::begin, std::span<T,Extent>::cbegin

```cpp
constexpr iterator begin() const noexcept;  // (1) (since C++20)
constexpr const_iterator cbegin() const noexcept;  // (2) (since C++23)
```

Returns an iterator to the first element of the `span`.

If the `span` is empty, the returned iterator will be equal to `end()`.

### Parameters

(none)

### Return value

Iterator to the first element.

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

- **endcend (C++23)** — returns an iterator to the end (public member function)
- **begincbegin (C++11)(C++14)** — returns an iterator to the beginning of a
  container or array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/span/begin*
