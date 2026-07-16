# std::map<Key,T,Compare,Allocator>::try_emplace

```cpp
template< class... Args >
std::pair<iterator, bool> try_emplace( const Key& k, Args&&... args );  // (1) (since C++17)
template< class... Args >
std::pair<iterator, bool> try_emplace( Key&& k, Args&&... args );  // (2) (since C++17)
template< class... Args >
iterator try_emplace( const_iterator hint, const Key& k, Args&&... args );  // (3) (since C++17)
template< class... Args >
iterator try_emplace( const_iterator hint, Key&& k, Args&&... args );  // (4) (since C++17)
```

Inserts a new element into the container with key `k` and value constructed with
`args`, if there is no element with the key in the container.

1) If a key equivalent to `k` already exists in the container, does nothing.
   Otherwise, behaves like `emplace` except that the element is constructed as
   `value_type(std::piecewise_construct, std::forward_as_tuple(k),
   std::forward_as_tuple (std::forward<Args>(args)...))`

2) If a key equivalent to `k` already exists in the container, does nothing.
   Otherwise, behaves like `emplace` except that the element is constructed as
   `value_type(std::piecewise_construct, std::forward_as_tuple(std::move(k)),
   std::forward_as_tuple (std::forward<Args>(args)...))`

3) If a key equivalent to `k` already exists in the container, does nothing.
   Otherwise, behaves like `emplace_hint` except that the element is constructed
   as `value_type(std::piecewise_construct, std::forward_as_tuple(k),
   std::forward_as_tuple (std::forward<Args>(args)...))`

4) If a key equivalent to `k` already exists in the container, does nothing.
   Otherwise, behaves like `emplace_hint` except that the element is constructed
   as `value_type(std::piecewise_construct, std::forward_as_tuple(std::move(k)),
   std::forward_as_tuple (std::forward<Args>(args)...))`

No iterators or references are invalidated.

### Parameters

- **k** — the key used both to look up and to insert if not found
- **hint** — iterator to the position before which the new element will be
  inserted
- **args** — arguments to forward to the constructor of the element

### Return value

1,2) Same as for `emplace`.

3,4) Same as for `emplace_hint`.

### Complexity

1,2) Same as for `emplace`.

3,4) Same as for `emplace_hint`.

### Notes

Unlike `insert` or `emplace`, these functions do not move from rvalue arguments
if the insertion does not happen, which makes it easy to manipulate maps whose
values are move-only types, such as `std::map<std::string,
std::unique_ptr<foo>>`. In addition, `try_emplace` treats the key and the
arguments to the `mapped_type` separately, unlike `emplace`, which requires the
arguments to construct a `value_type` (that is, a `std::pair`).

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_map_try_emplace` | 201411L | (C++17) | `std::map::try_emplace`,
      `std::map::insert_or_assign`

### Example

```cpp
#include <iostream>
#include <string>
#include <utility>
#include <map>

void print_node(const auto& node)
{
    std::cout << '[' << node.first << "] = " << node.second << '\n';
}

void print_result(auto const& pair)
{
    std::cout << (pair.second ? "inserted: " : "ignored:  ");
    print_node(*pair.first);
}

int main()
{
    using namespace std::literals;
    std::map<std::string, std::string> m;

    print_result(m.try_emplace("a", "a"s));
    print_result(m.try_emplace("b", "abcd"));
    print_result(m.try_emplace("c", 10, 'c'));
    print_result(m.try_emplace("c", "Won't be inserted"));

    for (const auto& p : m)
        print_node(p);
}
```

Output:

```text
inserted: [a] = a
inserted: [b] = abcd
inserted: [c] = cccccccccc
ignored:  [c] = cccccccccc
[a] = a
[b] = abcd
[c] = cccccccccc
```

### See also

- **emplace (C++11)** — constructs element in-place (public member function)
- **emplace_hint (C++11)** — constructs elements in-place using a hint (public
  member function)
- **insert** — inserts elements or nodes(since C++17) (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/map/try_emplace*
