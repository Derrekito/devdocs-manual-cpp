# std::vector<T,Allocator>::rbegin, std::vector<T,Allocator>::crbegin

```cpp
iterator rbegin();  // (until C++11)
iterator rbegin() noexcept;  // (since C++11) (constexpr since C++20)
const_iterator rbegin() const;  // (until C++11)
const_iterator rbegin() const noexcept;  // (since C++11) (constexpr since C++20)
const_iterator crbegin() const noexcept;  // (3) (since C++11) (constexpr since C++20)
```

Returns a reverse iterator to the first element of the reversed `vector`. It
corresponds to the last element of the non-reversed `vector`. If the `vector` is
empty, the returned iterator is equal to `rend()`.

### Parameters

(none)

### Return value

Reverse iterator to the first element.

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
    std::for_each(nums.rbegin(), nums.rend(), [](const int n) { std::cout << n << ' '; });
    std::cout << '\n';

    // Sums all integers in the vector nums (if any), printing only the result.
    std::cout << "Sum of nums: "
              << std::accumulate(nums.rbegin(), nums.rend(), 0) << '\n';

    // Prints the first fruit in the vector fruits, checking if there is any.
    if (!fruits.empty())
        std::cout << "First fruit: " << *fruits.rbegin() << '\n';

    if (empty.rbegin() == empty.rend())
        std::cout << "vector 'empty' is indeed empty.\n";
}
```

Output:

```text
16 8 4 2 1
Sum of nums: 31
First fruit: raspberry
vector 'empty' is indeed empty.
```

### See also

- **rendcrend (C++11)** — returns a reverse iterator to the end (public member
  function)
- **rbegincrbegin (C++14)** — returns a reverse iterator to the beginning of a
  container or array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/rbegin*
