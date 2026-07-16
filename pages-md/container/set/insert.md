# std::set<Key,Compare,Allocator>::insert

Inserts one or more elements — but **does nothing** if an equivalent
key is already present (no throw, no overwrite: a `set` holds unique
keys). Every form returns enough information to tell whether the
insertion actually happened.

```cpp skip
s.insert(value);                    // copy/move a value_type
s.insert(hint, value);              // positional hint, same rules
s.insert(first, last);              // range of value_type-likes
s.insert(ilist);                    // initializer_list
s.insert(std::move(nh));            // node handle              (C++17)
s.insert(hint, std::move(nh));      // node handle + hint        (C++17)
```

### What you provide

- **value** — the `value_type` (i.e. `Key`) to insert.
- **hint** — an iterator suggesting where to insert. A correct hint
  (the element belongs just before it) makes insertion amortized
  O(1); a wrong hint still works, just at the usual O(log n).
- **first, last** — an input-iterator range of `value_type`-likes.
  Duplicate keys within the range are unspecified as to which wins.
- **ilist** — a `std::initializer_list<value_type>`; same duplicate
  rule as the range form.
- **nh** — a node handle, e.g. from another set's `extract()`. An
  empty handle is a no-op; the allocators must match.

### Guarantees and costs

- Single-element insert: O(log n). Hinted insert: amortized O(1) if
  the hint is correct, O(log n) otherwise. Range/list insert:
  `O(N·log(size() + N))` for `N` inserted elements.
- No iterators or references are invalidated by insertion — a `set`'s
  nodes never move once placed.
- If an exception is thrown during a single/hinted insert, the
  insertion has no effect.
- (C++17) Inserting a node handle successfully invalidates any
  pointers/references obtained while it was held by the handle; ones
  obtained before it was extracted become valid again.

### Gotchas

- A failed insert (key already present) is silent — always check the
  returned `bool` if the outcome matters: `s.insert(v).second`.
- The hinted overloads return only an iterator, not a `pair` — to
  detect success there, compare `size()` before and after.
- The range/list overloads (5,6) are often implemented as a loop that
  calls the hinted overload with `end()`; they're optimized for
  appending an already-sorted sequence whose smallest element is
  greater than the set's current last element.

### Example

```cpp
#include <cassert>
#include <iostream>
#include <set>

int main()
{
    std::set<int> s;

    auto [it1, ok1] = s.insert(3);
    assert(*it1 == 3);
    if (ok1)
        std::cout << "insert done\n";

    auto [it2, ok2] = s.insert(3);
    assert(it2 == it1);   // same element, no new node
    if (!ok2)
        std::cout << "no insertion\n";
}
```

```text
insert done
no insertion
```

### Reference

```cpp skip
std::pair<iterator, bool> insert( const value_type& value );  // (1)
std::pair<iterator, bool> insert( value_type&& value );  // (2) (since C++11)
iterator insert( const_iterator pos, const value_type& value );  // (3)
iterator insert( const_iterator pos, value_type&& value );  // (4) (since C++11)
template< class InputIt >
void insert( InputIt first, InputIt last );  // (5)
void insert( std::initializer_list<value_type> ilist );  // (6) (since C++11)
insert_return_type insert( node_type&& nh );  // (7) (since C++17)
iterator insert( const_iterator pos, node_type&& nh );  // (8) (since C++17)
```

`InputIt` must meet LegacyInputIterator. Overload (7) returns an
`insert_return_type` with `inserted`, `position`, and `node` members
describing whether the insertion took place; overload (8) returns the
position iterator directly. The behavior is undefined if `nh` is
non-empty and its allocator differs from the container's. Two earlier
defects (LWG 233, LWG 316) are folded into the guarantees above: the
hint is honored as a real position hint, not just ignored, and success
is indicated by `true`.

### See also

- **emplace** — constructs element in-place (C++11)
- **emplace_hint** — same, with a positional hint (C++11)
- **erase** — removes elements
- **find** — locates an element by key
- **inserter** — creates a `std::insert_iterator` of the right type

---
*Source: https://en.cppreference.com/w/cpp/container/set/insert*
