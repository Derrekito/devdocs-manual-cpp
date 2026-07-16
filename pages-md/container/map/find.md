# std::map<Key,T,Compare,Allocator>::find

Looks up the element with the given key and returns an iterator to it
— or `end()` if there's no such element. There's no exception and no
special "not found" value other than `end()`, so the result must
always be checked before dereferencing.

```cpp skip
m.find(key);      // iterator, or end() on miss
m.find(x);         // heterogeneous lookup, needs transparent Compare (C++14)
```

### What you provide

- **key** — the key to look up, compared with `Compare`.
- **x** — a value of any type comparable to `Key` without converting
  it first. This overload only exists if the map's `Compare` (e.g.
  `std::less<>`) has a `Compare::is_transparent` member type
  (C++14) — it avoids constructing a temporary `Key` just to search.

### Guarantees and costs

- O(log n) — same as any other lookup on `map`.
- Returns `iterator` (or `const_iterator` on a const map) to the
  matching element, or `end()` if no element has that key.

### Gotchas

- Dereferencing the result without checking for `end()` first is
  undefined behavior — this is the single most common bug with
  `find`. The standard idiom, using a C++17 init-statement `if`, keeps
  the lookup scoped to the branch that uses it:
  `if (auto it = m.find(k); it != m.end()) { ... }`.
- `find` only ever returns zero or one element on `map` (keys are
  unique) — for `multimap`, pair it with `equal_range` instead.
- The transparent overload requires the map itself to be declared with
  a transparent comparator, e.g. `std::map<K, V, std::less<>>` — a
  map using the default `std::less<K>` doesn't get it, and passing an
  `x` of the wrong type will construct a temporary `Key` instead.

### Example

```cpp
#include <iostream>
#include <map>

int main()
{
    std::map<int, char> m{{1, 'a'}, {2, 'b'}};

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
iterator find( const K& x );  // (3) (since C++14)
template< class K >
const_iterator find( const K& x ) const;  // (4) (since C++14)
```

Overloads (3,4) participate in overload resolution only if
`Compare::is_transparent` is a valid type.

### See also

- **count** — number of elements matching a key (0 or 1 on `map`)
- **contains** — checks for a key without returning an iterator (C++20)
- **equal_range** — range of elements matching a key
- **at** — bounds-checked access to the mapped value
- **operator[]** — access, inserting a default value on miss

---
*Source: https://en.cppreference.com/w/cpp/container/map/find*
