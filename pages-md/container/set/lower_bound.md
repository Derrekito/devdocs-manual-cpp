# std::set<Key,Compare,Allocator>::lower_bound

Returns an iterator to the first element that is *not less than*
`key` — i.e. the first element that could equal it, or `end()` if
every element is less than `key`. This member function runs in
O(log n) because it walks the set's tree structure directly; calling
the free algorithm `std::lower_bound` with a set's iterators instead
would be much slower, since it can't see the tree and has to advance
its iterators one step at a time.

```cpp skip
s.lower_bound(key);     // first element >= key, or end()
s.lower_bound(x);   // heterogeneous lookup, needs transparent Compare (C++14)
```

### What you provide

- **key** — the key to compare elements against.
- **x** — a value of any type comparable to `Key` without converting
  it first. Requires `Compare::is_transparent` (C++14), same rule as
  `find`'s transparent overload.

### Guarantees and costs

- O(log n) — the member function exploits the tree; use it, not
  `std::lower_bound(s.begin(), s.end(), key)`.
- Returns `iterator` (or `const_iterator` on a const set) to the first
  element not less than `key`, or `end()` if none qualifies.

### Gotchas

- `std::lower_bound`, the free algorithm, needs random-access
  iterators to achieve O(log n); on a `set`'s bidirectional iterators
  it still does O(log n) comparisons but O(n) iterator increments to
  get there, since it has no way to jump. Always prefer the member
  function on associative containers.
- `lower_bound(key)` finds the first element *not less than* `key`,
  which includes an exact match — it is not "strictly greater than".
  Pair with `upper_bound` to get a range that excludes an exact match,
  or use `equal_range` for both bounds at once.
- The transparent overload requires the set itself to be declared with
  a transparent comparator, e.g. `std::set<K, std::less<>>`.

### Example

```cpp
#include <iostream>
#include <set>

int main()
{
    std::set<int> s{10, 20, 30, 40};

    auto it = s.lower_bound(25);
    std::cout << (it != s.end() ? *it : -1) << '\n';

    it = s.lower_bound(30);
    std::cout << (it != s.end() ? *it : -1) << '\n';   // exact match included

    it = s.lower_bound(100);
    std::cout << (it == s.end()) << '\n';
}
```

```text
30
30
1
```

### Reference

```cpp skip
iterator lower_bound( const Key& key );  // (1)
const_iterator lower_bound( const Key& key ) const;  // (2)
template< class K >
iterator lower_bound( const K& x );  // (3) (since C++14)
template< class K >
const_iterator lower_bound( const K& x ) const;  // (4) (since C++14)
```

Overloads (3,4) participate in overload resolution only if
`Compare::is_transparent` is a valid type.

### See also

- **upper_bound** — first element greater than a key
- **equal_range** — both bounds in one call
- **find** — locates an exact match

---
*Source: https://en.cppreference.com/w/cpp/container/set/lower_bound*
