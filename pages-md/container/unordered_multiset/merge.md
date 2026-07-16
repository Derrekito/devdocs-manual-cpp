# std::unordered_multiset<Key,Hash,KeyEqual,Allocator>::merge

```cpp
template< class H2, class P2 >
void merge( std::unordered_set<Key, H2, P2, Allocator>& source );  // (1) (since C++17)
template< class H2, class P2 >
void merge( std::unordered_set<Key, H2, P2, Allocator>&& source );  // (2) (since C++17)
template< class H2, class P2 >
void merge( std::unordered_multiset<Key, H2, P2, Allocator>& source );  // (3) (since C++17)
template< class H2, class P2 >
void merge( std::unordered_multiset<Key, H2, P2, Allocator>&& source );  // (4) (since C++17)
```

Attempts to extract ("splice") each element in `source` and insert it into
`*this` using the hash function and key equality predicate of `*this`.

No elements are copied or moved, only the internal pointers of the container
nodes are repointed. All pointers and references to the transferred elements
remain valid, but now refer into `*this`, not into `source`. Iterators referring
to the transferred elements and all iterators referring to `*this` are
invalidated.

The behavior is undefined if `get_allocator() != source.get_allocator()`.

### Parameters

- **source** — compatible container to transfer the nodes from

### Return value

(none)

### Complexity

Average case O(N), worst case O(N * size() + N), where N is `source.size()`.

### Example

```cpp
#include <iostream>
#include <unordered_set>

// print out a container
template<class Os, class K>
Os& operator<<(Os& os, const std::unordered_multiset<K>& v)
{
    os << '[' << v.size() << "] {";
    bool o{};
    for (const auto& e : v)
        os << (o ? ", " : (o = 1, " ")) << e;
    return os << " }\n";
}

int main()
{
    std::unordered_multiset<char>
        p{'C', 'B', 'B', 'A'},
        q{'E', 'D', 'E', 'C'};

    std::cout << "p: " << p << "q: " << q;

    p.merge(q);

    std::cout << "p.merge(q);\n" << "p: " << p << "q: " << q;
}
```

Possible output:

```text
p: [4] { A, B, B, C }
q: [4] { C, D, E, E }
p.merge(q);
p: [8] { E, E, D, A, B, B, C, C }
q: [0] { }
```

### See also

- **extract (C++17)** — extracts nodes from the container (public member
  function)
- **insert** — inserts elements or nodes(since C++17) (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_multiset/merge*
