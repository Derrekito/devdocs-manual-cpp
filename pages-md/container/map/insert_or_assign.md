# std::map<Key,T,Compare,Allocator>::insert_or_assign

```cpp
template< class M >
std::pair<iterator, bool> insert_or_assign( const Key& k, M&& obj );  // (1) (since C++17)
template< class M >
std::pair<iterator, bool> insert_or_assign( Key&& k, M&& obj );  // (2) (since C++17)
template< class M >
iterator insert_or_assign( const_iterator hint, const Key& k, M&& obj );  // (3) (since C++17)
template< class M >
iterator insert_or_assign( const_iterator hint, Key&& k, M&& obj );  // (4) (since C++17)
```

1,3) If a key equivalent to `k` already exists in the container, assigns
   `std::forward<M>(obj)` to the `mapped_type` corresponding to the key `k`. If
   the key does not exist, inserts the new value as if by `insert`, constructing
   it from `value_type(k, std::forward<M>(obj))`.

2,4) Same as (1,3), except the mapped value is constructed from
   `value_type(std::move(k), std::forward<M>(obj))`.

The behavior is undefined(until C++20)The program is ill-formed(since C++20) if
`std::is_assignable_v<mapped_type&, M&&>` is `false`.

No iterators or references are invalidated.

### Parameters

- **k** — the key used both to look up and to insert if not found
- **hint** — iterator to the position before which the new element will be
  inserted
- **obj** — the value to insert or assign

### Return value

1,2) The bool component is `true` if the insertion took place and `false` if the
   assignment took place. The iterator component is pointing at the element that
   was inserted or updated.

3,4) Iterator pointing at the element that was inserted or updated.

### Complexity

1,2) Same as for `emplace`.

3,4) Same as for `emplace_hint`.

### Notes

`insert_or_assign` returns more information than `operator[]` and does not
require default-constructibility of the mapped type.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_map_try_emplace` | 201411L | (C++17) | `std::map::try_emplace`,
      `std::map::insert_or_assign`

### Example

```cpp
#include <iostream>
#include <string>
#include <map>

void print_node(const auto& node)
{
    std::cout << '[' << node.first << "] = " << node.second << '\n';
}

void print_result(auto const& pair)
{
    std::cout << (pair.second ? "inserted: " : "assigned: ");
    print_node(*pair.first);
}

int main()
{
    std::map<std::string, std::string> myMap;

    print_result(myMap.insert_or_assign("a", "apple"));
    print_result(myMap.insert_or_assign("b", "banana"));
    print_result(myMap.insert_or_assign("c", "cherry"));
    print_result(myMap.insert_or_assign("c", "clementine"));

    for (const auto& node : myMap)
        print_node(node);
}
```

Output:

```text
inserted: [a] = apple
inserted: [b] = banana
inserted: [c] = cherry
assigned: [c] = clementine
[a] = apple
[b] = banana
[c] = clementine
```

### See also

- **operator[]** — access or insert specified element (public member function)
- **at** — access specified element with bounds checking (public member
  function)
- **insert** — inserts elements or nodes(since C++17) (public member function)
- **emplace (C++11)** — constructs element in-place (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/map/insert_or_assign*
