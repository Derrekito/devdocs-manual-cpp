# std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::emplace

```cpp
template< class... Args >
std::pair<iterator, bool> emplace( Args&&... args );  // (since C++11)
```

Inserts a new element into the container constructed in-place with the given
`args` if there is no element with the key in the container.

Careful use of `emplace` allows the new element to be constructed while avoiding
unnecessary copy or move operations. The constructor of the new element (i.e.
`std::pair<const Key, T>`) is called with exactly the same arguments as supplied
to `emplace`, forwarded via `std::forward<Args>(args)...`. The element may be
constructed even if there already is an element with the key in the container,
in which case the newly constructed element will be destroyed immediately.

If after the operation the new number of elements is greater than old
`max_load_factor() * bucket_count()` a rehashing takes place.
If rehashing occurs (due to the insertion), all iterators are invalidated.
Otherwise (no rehashing), iterators are not invalidated.

### Parameters

- **args** — arguments to forward to the constructor of the element

### Return value

Returns a pair consisting of an iterator to the inserted element, or the
already-existing element if no insertion happened, and a bool denoting whether
the insertion took place (true if insertion happened, false if it did not).

### Exceptions

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

### Complexity

Amortized constant on average, worst case linear in the size of the container.

### Example

```cpp
#include <iostream>
#include <string>
#include <unordered_map>
#include <utility>

int main()
{
    std::unordered_map<std::string, std::string> m;

    // uses pair's move constructor
    m.emplace(std::make_pair(std::string("a"), std::string("a")));

    // uses pair's converting move constructor
    m.emplace(std::make_pair("b", "abcd"));

    // uses pair's template constructor
    m.emplace("d", "ddd");

    // uses pair's piecewise constructor
    m.emplace(std::piecewise_construct,
              std::forward_as_tuple("c"),
              std::forward_as_tuple(10, 'c'));
    // as of C++17, m.try_emplace("c", 10, 'c'); can be used

    for (const auto& p : m)
        std::cout << p.first << " => " << p.second << '\n';
}
```

Possible output:

```text
a => a
b => abcd
c => cccccccccc
d => ddd
```

### See also

- **emplace_hint** — constructs elements in-place using a hint (public member
  function)
- **try_emplace (C++17)** — inserts in-place if the key does not exist, does
  nothing if the key exists (public member function)
- **insert** — inserts elements or nodes(since C++17) (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_map/emplace*
