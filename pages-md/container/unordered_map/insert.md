# std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::insert

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
- **hint** — an iterator, used only as a non-binding suggestion of
  where to start searching; unlike on `map`, it does not change the
  complexity here.
- **first, last** — an input-iterator range of `value_type`-likes.
  Duplicate keys within the range are unspecified as to which wins.
  Neither iterator may point into `*this`.
- **ilist** — a `std::initializer_list<value_type>`; same duplicate
  rule as the range form.
- **nh** — a node handle, e.g. from another `unordered_map`'s
  `extract()`. An empty handle is a no-op; the allocators must match.

### Guarantees and costs

- Single/hinted/node insert: average O(1), worst case O(size()) — the
  worst case comes from hash collisions forcing a bucket scan.
- Range/list insert: average O(N) for `N` inserted elements, worst
  case `O(N · size() + N)`.
- If the resulting element count exceeds
  `max_load_factor() * bucket_count()`, the container rehashes. A
  rehash invalidates **all** iterators; without one, iterators are
  left alone.
- If an exception is thrown by any single/hinted/node-handle insert,
  that call has no effect. The range and initializer-list overloads
  give no exception-safety guarantee at all.
- (C++17) Inserting a node handle successfully invalidates any
  pointers/references obtained while it was held by the handle; ones
  obtained before it was extracted become valid again.

### Gotchas

- A failed insert (key already present) is silent — always check the
  returned `bool` if the outcome matters: `m.insert({k, v}).second`.
- The hinted overloads return only an iterator, not a `pair` — to
  detect success there, compare `size()` before and after.
- A rehash can happen on *any* successful insert, not just large ones
  — any iterator you're holding into the map may die. If you need one
  to survive, re-`find` it afterward, or call `reserve()` up front to
  avoid the rehash entirely.
- Reaching for `insert` just to update an existing key is a bug in
  waiting: it silently does nothing. Use `operator[]` or `insert_or_assign`
  (C++17) when the key may already be there.

### Example

```cpp
#include <iostream>
#include <string>
#include <unordered_map>

int main()
{
    std::unordered_map<int, std::string> dict = {{1, "one"}, {2, "two"}};
    dict.insert({3, "three"});

    bool ok = dict.insert({1, "another one"}).second;
    std::cout << "inserting 1 => \"another one\" "
              << (ok ? "succeeded" : "failed") << '\n';

    std::cout << "size: " << dict.size() << '\n';
}
```

```text
inserting 1 => "another one" failed
size: 3
```

### Reference

```cpp skip
std::pair<iterator,bool> insert( const value_type& value );  // (since C++11)
std::pair<iterator,bool> insert( value_type&& value );  // (since C++17)
template< class P >
std::pair<iterator,bool> insert( P&& value );  // (since C++11)
iterator insert( const_iterator hint, const value_type& value );  // (since C++11)
iterator insert( const_iterator hint, value_type&& value );  // (since C++17)
template< class P >
iterator insert( const_iterator hint, P&& value );  // (since C++11)
template< class InputIt >
void insert( InputIt first, InputIt last );  // (since C++11)
void insert( std::initializer_list<value_type> ilist );  // (since C++11)
insert_return_type insert( node_type&& nh );  // (since C++17)
iterator insert( const_iterator hint, node_type&& nh );  // (since C++17)
```

The `P&&` overloads are equivalent to `emplace(std::forward<P>(value))`
(and its hinted counterpart `emplace_hint`), participating in overload
resolution only if `value_type` is constructible from `P&&`. The node
handle overload returns an `insert_return_type` with `inserted`,
`position`, and `node` members describing whether the insertion took
place. An earlier defect (LWG 2005) is folded into the wording above:
the `P&&` overloads require `value_type` to be constructible from
`P&&`, not merely that `P` convert to it.

### See also

- **emplace** — constructs element in-place
- **emplace_hint** — same, with a positional hint
- **insert_or_assign** — inserts, or assigns if the key exists (C++17)
- **erase** — removes elements
- **find** — looks up an element by key

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_map/insert*
