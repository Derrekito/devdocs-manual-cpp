# std::unordered_multimap<Key,T,Hash,KeyEqual,Allocator>::emplace

```cpp
template< class... Args >
iterator emplace( Args&&... args );  // (since C++11)
```

Inserts a new element into the container constructed in-place with the given
`args` .

Careful use of `emplace` allows the new element to be constructed while avoiding
unnecessary copy or move operations. The constructor of the new element (i.e.
`std::pair<const Key, T>`) is called with exactly the same arguments as supplied
to `emplace`, forwarded via `std::forward<Args>(args)...`.

If after the operation the new number of elements is greater than old
`max_load_factor() * bucket_count()` a rehashing takes place.
If rehashing occurs (due to the insertion), all iterators are invalidated.
Otherwise (no rehashing), iterators are not invalidated.

### Parameters

- **args** — arguments to forward to the constructor of the element

### Return value

Returns an iterator to the inserted element.

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
    std::unordered_multimap<std::string, std::string> m;

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
- **try_emplace** — inserts in-place if the key does not exist, does nothing if
  the key exists (public member function)
- **insert** — inserts elements or nodes(since C++17) (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_multimap/emplace*
