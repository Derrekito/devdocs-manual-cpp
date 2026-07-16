# std::list<T,Allocator>::end, std::list<T,Allocator>::cend

```cpp
iterator end();  // (until C++11)
iterator end() noexcept;  // (since C++11)
const_iterator end() const;  // (until C++11)
const_iterator end() const noexcept;  // (since C++11)
const_iterator cend() const noexcept;  // (3) (since C++11)
```

Returns an iterator to the element following the last element of the `list`.

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
#include <algorithm>
#include <iostream>
#include <numeric>
#include <string>
#include <list>

int main()
{
    std::list<int> nums{1, 2, 4, 8, 16};
    std::list<std::string> fruits{"orange", "apple", "raspberry"};
    std::list<char> empty;

    // Print list.
    std::for_each(nums.begin(), nums.end(), [](const int n) { std::cout << n << ' '; });
    std::cout << '\n';

    // Sums all integers in the list nums (if any), printing only the result.
    std::cout << "Sum of nums: "
              << std::accumulate(nums.begin(), nums.end(), 0) << '\n';

    // Prints the first fruit in the list fruits, checking if there is any.
    if (!fruits.empty())
        std::cout << "First fruit: " << *fruits.begin() << '\n';

    if (empty.begin() == empty.end())
        std::cout << "list 'empty' is indeed empty.\n";
}
```

Output:

```text
1 2 4 8 16
Sum of nums: 31
First fruit: orange
list 'empty' is indeed empty.
```

### See also

- **begincbegin (C++11)** — returns an iterator to the beginning (public member
  function)
- **endcend (C++11)(C++14)** — returns an iterator to the end of a container or
  array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/list/end*
