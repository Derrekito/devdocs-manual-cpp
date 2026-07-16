# std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::find

Looks up the element with the given key and returns an iterator to it
— or `end()` if there's no such element. There's no exception and no
special "not found" value other than `end()`, so the result must
always be checked before dereferencing.

```cpp skip
m.find(key);      // iterator, or end() on miss
m.find(x);    // heterogeneous lookup, needs transparent Hash/KeyEqual (C++20)
```

### What you provide

- **key** — the key to look up, compared with `KeyEqual` after
  hashing with `Hash`.
- **x** — a value of any type transparently comparable to `Key`. This
  overload only exists if both `Hash::is_transparent` and
  `KeyEqual::is_transparent` are valid member types (C++20) — `Hash`
  must be callable with both `K` and `Key`, avoiding a temporary
  `Key` just to search.

### Guarantees and costs

- Constant on average; worst case linear in the size of the container
  (all elements landing in one bucket, e.g. from a poor `Hash`).
- Returns `iterator` (or `const_iterator` on a const map) to the
  matching element, or `end()` if no element has that key.

### Gotchas

- Dereferencing the result without checking for `end()` first is
  undefined behavior — the standard idiom, using a C++17
  init-statement `if`, keeps the lookup scoped to the branch that
  uses it: `if (auto it = m.find(k); it != m.end()) { ... }`.
- `find` only ever returns zero or one element on `unordered_map`
  (keys are unique).
- The transparent overload requires *both* `Hash` and `KeyEqual` to be
  declared transparent, e.g. a custom hash with
  `using is_transparent = void;` paired with `std::equal_to<>` — the
  default `std::hash<Key>` does not qualify, and passing an `x` of the
  wrong type silently constructs a temporary `Key` instead.

### Example

```cpp
#include <iostream>
#include <unordered_map>

int main()
{
    std::unordered_map<int, char> m{{1, 'a'}, {2, 'b'}};

    if (auto it = m.find(2); it != m.end())
        std::cout << "found " << it->first << ' ' << it->second << '\n';
    else
        std::cout << "not found\n";

    if (auto it = m.find(9); it == m.end())
        std::cout << "9 not found\n";
}
```

```text
found 2 b
9 not found
```

### Reference

```cpp skip
iterator find( const Key& key );  // (1)
const_iterator find( const Key& key ) const;  // (2)
template< class K >
iterator find( const K& x );  // (3) (since C++20)
template< class K >
const_iterator find( const K& x ) const;  // (4) (since C++20)
```

Overloads (3,4) participate in overload resolution only if
`Hash::is_transparent` and `KeyEqual::is_transparent` are both valid
types.

### See also

- **count** — number of elements matching a key (0 or 1)
- **contains** — checks for a key without returning an iterator (C++20)
- **equal_range** — range of elements matching a key
- **at** — bounds-checked access to the mapped value
- **operator[]** — access, inserting a default value on miss

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_map/find*
