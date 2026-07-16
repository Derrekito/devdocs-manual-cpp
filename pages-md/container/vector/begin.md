# std::vector<T,Allocator>::begin, std::vector<T,Allocator>::cbegin

```cpp
iterator begin();  // (until C++11)
iterator begin() noexcept;  // (since C++11) (constexpr since C++20)
const_iterator begin() const;  // (until C++11)
const_iterator begin() const noexcept;  // (since C++11) (constexpr since C++20)
const_iterator cbegin() const noexcept;  // (3) (since C++11) (constexpr since C++20)
```

Returns an iterator to the first element of the `vector`.

If the `vector` is empty, the returned iterator will be equal to `end()`.

### Parameters

(none)

### Return value

Iterator to the first element.

### Complexity

Constant.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <numeric>
#include <string>
#include <vector>

int main()
{
    std::vector<int> nums{1, 2, 4, 8, 16};
    std::vector<std::string> fruits{"orange", "apple", "raspberry"};
    std::vector<char> empty;

    // Print vector.
    std::for_each(nums.begin(), nums.end(), [](const int n) { std::cout << n << ' '; });
    std::cout << '\n';

    // Sums all integers in the vector nums (if any), printing only the result.
    std::cout << "Sum of nums: "
              << std::accumulate(nums.begin(), nums.end(), 0) << '\n';

    // Prints the first fruit in the vector fruits, checking if there is any.
    if (!fruits.empty())
        std::cout << "First fruit: " << *fruits.begin() << '\n';

    if (empty.begin() == empty.end())
        std::cout << "vector 'empty' is indeed empty.\n";
}
```

Output:

```text
1 2 4 8 16
Sum of nums: 31
First fruit: orange
vector 'empty' is indeed empty.
```

### See also

- **endcend (C++11)** — returns an iterator to the end (public member function)
- **begincbegin (C++11)(C++14)** — returns an iterator to the beginning of a
  container or array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/begin*
