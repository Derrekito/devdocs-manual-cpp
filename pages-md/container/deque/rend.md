# std::deque<T,Allocator>::rend, std::deque<T,Allocator>::crend

```cpp
reverse_iterator rend();  // (until C++11)
reverse_iterator rend() noexcept;  // (since C++11)
const_reverse_iterator rend() const;  // (until C++11)
const_reverse_iterator rend() const noexcept;  // (since C++11)
const_reverse_iterator crend() const noexcept;  // (3) (since C++11)
```

Returns a reverse iterator to the element following the last element of the
reversed `deque`. It corresponds to the element preceding the first element of
the non-reversed `deque`. This element acts as a placeholder, attempting to
access it results in undefined behavior.

### Parameters

(none)

### Return value

Reverse iterator to the element following the last element.

### Complexity

Constant.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <numeric>
#include <string>
#include <deque>

int main()
{
    std::deque<int> nums{1, 2, 4, 8, 16};
    std::deque<std::string> fruits{"orange", "apple", "raspberry"};
    std::deque<char> empty;

    // Print deque.
    std::for_each(nums.rbegin(), nums.rend(), [](const int n) { std::cout << n << ' '; });
    std::cout << '\n';

    // Sums all integers in the deque nums (if any), printing only the result.
    std::cout << "Sum of nums: "
              << std::accumulate(nums.rbegin(), nums.rend(), 0) << '\n';

    // Prints the first fruit in the deque fruits, checking if there is any.
    if (!fruits.empty())
        std::cout << "First fruit: " << *fruits.rbegin() << '\n';

    if (empty.rbegin() == empty.rend())
        std::cout << "deque 'empty' is indeed empty.\n";
}
```

Output:

```text
16 8 4 2 1
Sum of nums: 31
First fruit: raspberry
deque 'empty' is indeed empty.
```

### See also

- **rbegincrbegin (C++11)** — returns a reverse iterator to the beginning
  (public member function)
- **rendcrend (C++14)** — returns a reverse end iterator for a container or
  array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/deque/rend*
