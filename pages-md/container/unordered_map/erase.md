# std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::erase

Removes elements. The iterator overloads return the iterator that
followed the removed element(s) — which is what makes the standard
erase-while-iterating loop work. The key overloads instead return a
count of how many elements were removed (`0` or `1`, since keys are
unique). The relative order of the elements that remain is preserved.

```cpp skip
m.erase(pos);          // remove one element, return the next iterator
m.erase(first, last);  // remove a range, return the iterator after it
m.erase(key);          // remove by key, return count removed (0 or 1)
m.erase(x);             // heterogeneous erase-by-key            (C++23)
```

### What you provide

- **pos** — a valid, dereferenceable iterator into this map (`end()`
  does not qualify).
- **first, last** — a valid `[first, last)` range within the map.
- **key** — the key of the element to remove, if present.
- **x** — a value transparently comparable to `Key`; requires both
  `Hash` and `KeyEqual` to be transparent (C++23), same rule as the
  transparent overload of `find`.

### Guarantees and costs

- `erase(pos)` / `erase(key)` / `erase(x)`: average O(1) (average
  `count(key)` for the key forms), worst case O(size()).
- `erase(first, last)`: average `std::distance(first, last)`, worst
  case O(size()).
- Only iterators and references to the *erased* elements are
  invalidated — every other iterator and reference stays valid across
  the call.
- The iterator overloads never throw. The key overloads may propagate
  an exception from `Hash` or `KeyEqual`.

### Gotchas

- The classic erase-while-iterating bug: after `m.erase(it)`, `it`
  itself is invalidated, so `++it` on the old value is undefined
  behavior. Always reassign: `it = m.erase(it);` and skip the `++it`
  on that branch.
- `erase(key)` on a missing key is a harmless no-op that returns `0`;
  it does not throw.
- `pos` must be dereferenceable — passing `end()` is undefined
  behavior, not a safe "erase nothing".
- Unlike `insert`, `erase` never triggers a rehash, so it never
  invalidates iterators beyond the ones pointing at what was removed.

### Example

```cpp
#include <iostream>
#include <string>
#include <unordered_map>

int main()
{
    std::unordered_map<int, std::string> m{
        {1, "one"}, {2, "two"}, {3, "three"}, {4, "four"}
    };

    for (auto it = m.begin(); it != m.end();)
    {
        if (it->first % 2 != 0)
            it = m.erase(it);   // reassign: it is now the next element
        else
            ++it;
    }

    std::cout << "size: " << m.size() << '\n';
    std::cout << "erase 2, erased count: " << m.erase(2) << '\n';
    std::cout << "erase 2, erased count: " << m.erase(2) << '\n';
}
```

```text
size: 2
erase 2, erased count: 1
erase 2, erased count: 0
```

### Reference

```cpp skip
iterator erase( iterator pos );  // (since C++11)
iterator erase( const_iterator pos );  // (since C++11)
iterator erase( const_iterator first, const_iterator last );  // (since C++11)
size_type erase( const Key& key );  // (since C++11)
template< class K >
size_type erase( K&& x );  // (since C++23)
```

The `K&&` overload participates in overload resolution only if
`Hash::is_transparent` and `KeyEqual::is_transparent` are both valid
and neither `iterator` nor `const_iterator` is implicitly convertible
from `K`. Two earlier defects (LWG 2059, LWG 2356) are folded into the
wording above: both `iterator` and `const_iterator` `pos` overloads
exist without ambiguity, and the order of the remaining elements is
guaranteed to be preserved.

### See also

- **clear** — removes all elements
- **find** — locates an element by key
- **extract** — removes an element without destroying it (C++17)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_map/erase*
