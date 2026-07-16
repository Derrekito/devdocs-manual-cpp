# std::multimap<Key,T,Compare,Allocator>::rbegin, std::multimap<Key,T,Compare,Allocator>::crbegin

```cpp
reverse_iterator rbegin();  // (until C++11)
reverse_iterator rbegin() noexcept;  // (since C++11)
const_reverse_iterator rbegin() const;  // (until C++11)
const_reverse_iterator rbegin() const noexcept;  // (since C++11)
const_reverse_iterator crbegin() const noexcept;  // (3) (since C++11)
```

Returns a reverse iterator to the first element of the reversed `multimap`. It
corresponds to the last element of the non-reversed `multimap`. If the
`multimap` is empty, the returned iterator is equal to `rend()`.

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
#include <map>
#include <string>

int main()
{
    std::multimap<std::string, int> multimap
    {
        {"█", 1},
        {"▒", 5},
        {"░", 3},
        {"▓", 7},
        {"▓", 8},
        {"░", 4},
        {"▒", 6},
        {"█", 2}
    };

    std::cout << "Print out in reverse order using const reverse iterators:\n";
    std::for_each(multimap.crbegin(), multimap.crend(),
        [](std::pair<const std::string, int> const& e)
        {
            std::cout << "{ \"" << e.first << "\", " << e.second << " };\n";
        });

    multimap.rbegin()->second = 42; // OK: non-const value is modifiable
//  multimap.crbegin()->second = 42; // Error: cannot modify the const value
}
```

Possible output:

```text
Print out in reverse order using const reverse iterators:
{ "▓", 8 };
{ "▓", 7 };
{ "▒", 6 };
{ "▒", 5 };
{ "░", 4 };
{ "░", 3 };
{ "█", 2 };
{ "█", 1 };
```

### See also

- **rendcrend (C++11)** — returns a reverse iterator to the end (public member
  function)
- **rbegincrbegin (C++14)** — returns a reverse iterator to the beginning of a
  container or array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/multimap/rbegin*
