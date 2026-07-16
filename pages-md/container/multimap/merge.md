# std::multimap<Key,T,Compare,Allocator>::merge

```cpp
template< class C2 >
void merge( std::map<Key, T, C2, Allocator>& source );  // (1) (since C++17)
template< class C2 >
void merge( std::map<Key, T, C2, Allocator>&& source );  // (2) (since C++17)
template< class C2 >
void merge( std::multimap<Key, T, C2, Allocator>& source );  // (3) (since C++17)
template< class C2 >
void merge( std::multimap<Key, T, C2, Allocator>&& source );  // (4) (since C++17)
```

Attempts to extract ("splice") each element in `source` and insert it into
`*this` using the comparison object of `*this`.

No elements are copied or moved, only the internal pointers of the container
nodes are repointed. All pointers and references to the transferred elements
remain valid, but now refer into `*this`, not into `source`.

The behavior is undefined if `get_allocator() != source.get_allocator()`.

### Parameters

- **source** — compatible container to transfer the nodes from

### Return value

(none)

### Exceptions

Does not throw unless comparison throws.

### Complexity

N * log(size() + N)), where N is `source.size()`.

### Example

```cpp
#include <iostream>
#include <map>
#include <string>

int main()
{
    std::multimap<int, std::string> ma{{1, "apple"}, {5, "pear"}, {10, "banana"}};
    std::multimap<int, std::string> mb{{2, "zorro"}, {4, "batman"}, {5, "X"}, {8, "alpaca"}};
    std::multimap<int, std::string> u;
    u.merge(ma);
    std::cout << "ma.size(): " << ma.size() << '\n';
    u.merge(mb);
    std::cout << "mb.size(): " << mb.size() << '\n';

    for (auto const& kv : u)
        std::cout << kv.first << ", " << kv.second << '\n';
}
```

Output:

```text
ma.size(): 0
mb.size(): 0
1, apple
2, zorro
4, batman
5, pear
5, X
8, alpaca
10, banana
```

### See also

- **extract (C++17)** — extracts nodes from the container (public member
  function)
- **insert** — inserts elements or nodes(since C++17) (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/multimap/merge*
