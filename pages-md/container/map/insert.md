# std::map<Key,T,Compare,Allocator>::insert

Inserts one or more elements — but **does nothing** if the key already
exists (no throw, no overwrite; use `operator[]` or `insert_or_assign`
for that). Every form returns enough information to tell whether the
insertion actually happened.

```cpp skip
m.insert({key, value});             // copy/move a value_type
m.insert(some_pair);                // convertible-to-value_type overload
m.insert(hint, {key, value});       // positional hint, same rules
m.insert(first, last);              // range of value_type-likes
m.insert(ilist);                    // initializer_list
m.insert(std::move(nh));            // node handle              (C++17)
m.insert(hint, std::move(nh));      // node handle + hint        (C++17)
```

### What you provide

- **value / some_pair** — a `std::pair<const Key, T>`, or anything
  convertible to it; forwarded as if to `emplace`.
- **hint** — an iterator suggesting where to insert. If it's the exact
  spot the element belongs, insertion is amortized O(1); a wrong hint
  still works, just at the usual O(log n).
- **first, last** — an input-iterator range of `value_type`-likes.
  Duplicate keys within the range are unspecified as to which wins.
- **ilist** — a `std::initializer_list<value_type>`; same duplicate
  rule as the range form.
- **nh** — a node handle, e.g. from another map's `extract()`. An
  empty handle is a no-op; the allocators must match.

### Guarantees and costs

- Single-element insert: O(log n). Hinted insert: amortized O(1) if
  the hint is correct, O(log n) otherwise. Range/list insert:
  `O(N·log(size() + N))` for `N` inserted elements.
- No iterators or references are invalidated by insertion — a `map`'s
  nodes never move once placed.
- If an exception is thrown during a single/hinted insert, the
  insertion has no effect.
- (C++17) Inserting a node handle successfully invalidates any
  pointers/references obtained while it was held by the handle; ones
  obtained before it was extracted become valid again.

### Gotchas

- A failed insert (key already present) is silent — always check the
  returned `bool` (or compare against the node handle's element) if
  the outcome matters. `m.insert({k, v}).second` is the idiom.
- The hinted overloads return only an iterator, not a `pair` — to
  detect success there, compare `size()` before and after.
- Reaching for `insert` just to update an existing key is a bug in
  waiting: it silently does nothing. Use `operator[]` or, since
  C++17, `insert_or_assign` when the key may already be there.
- `try_emplace` (C++17) is preferable to `insert` when constructing
  the value is expensive, since it only constructs on an actual miss.

### Example

```cpp
#include <iostream>
#include <map>
#include <string>

int main()
{
    std::map<std::string, int> ages;

    if (auto [it, ok] = ages.insert({"Ann", 30}); ok)
        std::cout << "inserted " << it->first << '\n';

    if (auto [it, ok] = ages.insert({"Ann", 99}); !ok)
        std::cout << "Ann already present, still " << it->second << '\n';

    ages.insert({{"Bo", 25}, {"Cy", 40}});   // initializer_list form

    for (const auto& [name, age] : ages)
        std::cout << name << ':' << age << ' ';
    std::cout << '\n';
}
```

```text
inserted Ann
Ann already present, still 30
Ann:30 Bo:25 Cy:40
```

### Reference

```cpp skip
std::pair<iterator, bool> insert( const value_type& value );  // (1)
template< class P >
std::pair<iterator, bool> insert( P&& value );  // (2) (since C++11)
std::pair<iterator, bool> insert( value_type&& value );  // (3) (since C++17)
iterator insert( iterator pos, const value_type& value );  // (until C++11)
iterator insert( const_iterator pos, const value_type& value );  // (since C++11)
template< class P >
iterator insert( const_iterator pos, P&& value );  // (5) (since C++11)
iterator insert( const_iterator pos, value_type&& value );  // (6) (since C++17)
template< class InputIt >
void insert( InputIt first, InputIt last );  // (7)
void insert( std::initializer_list<value_type> ilist );  // (8) (since C++11)
insert_return_type insert( node_type&& nh );  // (9) (since C++17)
iterator insert( const_iterator pos, node_type&& nh );  // (10) (since C++17)
```

Overload (2) is equivalent to `emplace(std::forward<P>(value))` and
participates in overload resolution only if `value_type` is
constructible from `P&&`; overload (5) is the hinted counterpart,
equivalent to `emplace_hint`. `InputIt` must meet
LegacyInputIterator. Overload (9) returns an `insert_return_type` with
`inserted`, `position`, and `node` members describing whether the
insertion took place; overload (10) returns the position iterator
directly. The behavior is undefined if `nh` is non-empty and its
allocator differs from the container's.

### See also

- **emplace** — constructs the element in place (C++11)
- **emplace_hint** — same, with a positional hint (C++11)
- **insert_or_assign** — inserts, or assigns if the key exists (C++17)
- **try_emplace** — constructs in place only if the key is missing (C++17)
- **erase** — removes elements
- **find** — looks up an element by key

---
*Source: https://en.cppreference.com/w/cpp/container/map/insert*
