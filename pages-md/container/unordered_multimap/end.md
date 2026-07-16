# std::unordered_multimap<Key,T,Hash,KeyEqual,Allocator>::end, std::unordered_multimap<Key,T,Hash,KeyEqual,Allocator>::cend

```cpp
iterator end() noexcept;  // (1) (since C++11)
const_iterator end() const noexcept;  // (2) (since C++11)
const_iterator cend() const noexcept;  // (3) (since C++11)
```

Returns an iterator to the element following the last element of the
`unordered_multimap`.

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
#include <cassert>
#include <iostream>
#include <string>
#include <utility>
#include <unordered_map>

int main()
{
    auto show_node = [](const std::pair<std::string, std::string>& node)
    {
        std::cout << node.first << " : " << node.second << '\n';
    };

    std::unordered_multimap<std::string, std::string> lemmas;
    assert(lemmas.begin() == lemmas.end());
    assert(lemmas.cbegin() == lemmas.cend());

    lemmas.insert({ "1. ∀x ∈ N ∃y ∈ N", "x ≤ y" });
    show_node(*lemmas.cbegin());
    assert(lemmas.begin() != lemmas.end());
    assert(lemmas.cbegin() != lemmas.cend());

    lemmas.begin()->second = "x < y";
    show_node(*lemmas.cbegin());

    lemmas.insert({ "2. ∀x, y ∈ N    ", "x = y V x ≠ y" });
    show_node(*lemmas.cbegin());

    lemmas.insert({ "3. ∀x ∈ N ∃y ∈ N", "y = x + 1" });
    show_node(*lemmas.cbegin());

    std::cout << "Lemmas: \n";
    std::for_each(lemmas.cbegin(), lemmas.cend(), [&](const auto& n)
    {
        show_node(n);
    });
    std::cout << '\n';
}
```

Possible output:

```text
1. ∀x ∈ N ∃y ∈ N : x ≤ y
1. ∀x ∈ N ∃y ∈ N : x < y
2. ∀x, y ∈ N     : x = y V x ≠ y
3. ∀x ∈ N ∃y ∈ N : y = x + 1
Lemmas:
3. ∀x ∈ N ∃y ∈ N : y = x + 1
1. ∀x ∈ N ∃y ∈ N : x < y
2. ∀x, y ∈ N     : x = y V x ≠ y
```

### See also

- **begincbegin** — returns an iterator to the beginning (public member
  function)
- **endcend (C++11)(C++14)** — returns an iterator to the end of a container or
  array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_multimap/end*
