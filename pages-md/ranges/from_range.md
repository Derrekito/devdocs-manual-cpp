# std::from_range, std::from_range_t

```cpp
struct from_range_t { explicit from_range_t() = default; };  // (since C++23)
inline constexpr std::from_range_t from_range {};  // (since C++23)
```

`std::from_range` is a disambiguation tag that can be passed to the constructors
of the suitable containers to indicate that the contained member is range
constructed.

The corresponding type `std::from_range_t` can be used in the constructor's
parameter list to match the intended tag.

### Standard library

The following standard library types use `std::from_range_t` type in their
constructors:

**Containers library**

- **(constructor) (C++23)** — constructs the `vector` from a range (public
  member function of `std::vector<T,Allocator>`)
- **(constructor) (C++23)** — constructs the `deque` from a range (public member
  function of `std::deque<T,Allocator>`)
- **(constructor) (C++23)** — constructs the `forward_list` from a range (public
  member function of `std::forward_list<T,Allocator>`)
- **(constructor) (C++23)** — constructs the `list` from a range (public member
  function of `std::list<T,Allocator>`)
- **(constructor) (C++23)** — constructs the `set` from a range (public member
  function of `std::set<Key,Compare,Allocator>`)
- **(constructor) (C++23)** — constructs the `map` from a range (public member
  function of `std::map<Key,T,Compare,Allocator>`)
- **(constructor) (C++23)** — constructs the `multiset` from a range (public
  member function of `std::multiset<Key,Compare,Allocator>`)
- **(constructor) (C++23)** — constructs the `multimap` from a range (public
  member function of `std::multimap<Key,T,Compare,Allocator>`)
- **(constructor) (C++23)** — constructs the `unordered_set` from a range
  (public member function of `std::unordered_set<Key,Hash,KeyEqual,Allocator>`)
- **(constructor) (C++23)** — constructs the `unordered_map` from a range
  (public member function of
  `std::unordered_map<Key,T,Hash,KeyEqual,Allocator>`)
- **(constructor) (C++23)** — constructs the `unordered_multiset` from a range
  (public member function of
  `std::unordered_multiset<Key,Hash,KeyEqual,Allocator>`)
- **(constructor) (C++23)** — constructs the `unordered_multimap` from a range
  (public member function of
  `std::unordered_multimap<Key,T,Hash,KeyEqual,Allocator>`)
- **(constructor) (C++23)** — constructs the `priority_queue` from a range
  (public member function of `std::priority_queue<T,Container,Compare>`)
- **(constructor) (C++23)** — constructs the `queue` from a range (public member
  function of `std::queue<T,Container>`)
- **(constructor) (C++23)** — constructs the `stack` from a range (public member
  function of `std::stack<T,Container>`)
- **(constructor) (C++23)** — constructs the `flat_set` from a range (public
  member function of `std::flat_set<Key,Compare,KeyContainer>`)
- **(constructor) (C++23)** — constructs the `flat_map` from a range (public
  member function of
  `std::flat_map<Key,T,Compare,KeyContainer,MappedContainer>`)
- **(constructor) (C++23)** — constructs the `flat_multiset` from a range
  (public member function of `std::flat_multiset<Key,Compare,KeyContainer>`)
- **(constructor) (C++23)** — constructs the `flat_multimap` from a range
  (public member function of
  `std::flat_multimap<Key,T,Compare,KeyContainer,MappedContainer>`)

**Strings library**

- **(constructor) (C++23)** — constructs the `basic_string` from a range (public
  member function of `std::basic_string<CharT,Traits,Allocator>`)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | Tagged constructors to
      construct from container compatible range

### Example

```cpp
#include <cassert>
#include <string>

int main()
{
#ifdef __cpp_lib_containers_ranges
    auto const range = {0x43, 43, 43};
    std::string str{std::from_range, range}; // uses tagged constructor
    assert(str == "C++");
#endif
}
```

### See also

- **ranges::to (C++23)** — constructs a new non-view object from an input range
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/from_range*
