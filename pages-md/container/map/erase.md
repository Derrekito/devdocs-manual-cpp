# std::map<Key,T,Compare,Allocator>::erase

Removes elements. The iterator overloads return the iterator that
followed the removed element(s) — which is what makes the standard
erase-while-iterating loop work. The key overloads instead return a
count of how many elements were removed (`0` or `1` on `map`, since
keys are unique).

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
- **x** — a value transparently comparable to `Key`; requires a
  transparent `Compare` (C++23), same rule as the transparent
  overloads of `find`.

### Guarantees and costs

- `erase(pos)`: amortized O(1).
- `erase(first, last)`: `O(log(size()) + std::distance(first, last))`.
- `erase(key)` / `erase(x)`: `O(log(size()) + count(key))`.
- Only iterators and references to the *erased* elements are
  invalidated — everything else in the map stays valid. This holds
  because `map` is node-based; nodes never move, so erasing one
  element can't disturb pointers into another (unlike, say,
  `std::vector::erase`).
- The iterator overloads never throw. The key overloads may propagate
  an exception from `Compare`.

### Gotchas

- The classic erase-while-iterating bug: after `m.erase(it)`, `it`
  itself is invalidated, so `++it` on the old value is undefined
  behavior. Always reassign: `it = m.erase(it);` and skip the `++it`
  on that branch — because only the *erased* element's iterator dies,
  every other iterator in the loop stays valid across the call.
- `erase(key)` on a missing key is a harmless no-op that returns `0`;
  it does not throw.
- `pos` must be dereferenceable — passing `end()` is undefined
  behavior, not a safe "erase nothing".

### Example

```cpp
#include <iostream>
#include <map>
#include <string>

int main()
{
    std::map<int, std::string> m{
        {1, "one"}, {2, "two"}, {3, "three"}, {4, "four"}
    };

    for (auto it = m.begin(); it != m.end();)
    {
        if (it->first % 2 != 0)
            it = m.erase(it);   // reassign: it is now the next element
        else
            ++it;
    }

    for (const auto& [k, v] : m)
        std::cout << v << ' ';
    std::cout << '\n';
}
```

```text
two four
```

### Reference

```cpp skip
iterator erase( iterator pos );  // (1)
iterator erase( const_iterator pos );  // (2) (since C++11)
iterator erase( iterator first, iterator last );  // (until C++11)
iterator erase( const_iterator first, const_iterator last );  // (since C++11)
size_type erase( const Key& key );  // (4)
template< class K >
size_type erase( K&& x );  // (5) (since C++23)
```

Overload (5) participates in overload resolution only if
`Compare::is_transparent` is valid and neither `iterator` nor
`const_iterator` is implicitly constructible from `K`.

### See also

- **clear** — removes all elements
- **extract** — removes an element without destroying it (C++17)
- **find** — locates an element by key

---
*Source: https://en.cppreference.com/w/cpp/container/map/erase*
