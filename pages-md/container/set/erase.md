# std::set<Key,Compare,Allocator>::erase

Removes elements. The iterator overloads return the iterator that
followed the removed element(s) — which is what makes the standard
erase-while-iterating loop work. The key overloads instead return a
count of how many elements were removed (`0` or `1`, since keys are
unique).

```cpp skip
s.erase(pos);          // remove one element, return the next iterator
s.erase(first, last);  // remove a range, return the iterator after it
s.erase(key);          // remove by key, return count removed (0 or 1)
s.erase(x);             // heterogeneous erase-by-key            (C++23)
```

### What you provide

- **pos** — a valid, dereferenceable iterator into this set (`end()`
  does not qualify).
- **first, last** — a valid `[first, last)` range within the set.
- **key** — the key of the element to remove, if present.
- **x** — a value transparently comparable to `Key`; requires a
  transparent `Compare` (C++23), same rule as the transparent
  overloads of `find`.

### Guarantees and costs

- `erase(pos)`: amortized O(1).
- `erase(first, last)`: `O(log(size()) + std::distance(first, last))`.
- `erase(key)` / `erase(x)`: `O(log(size()) + count(key))`.
- Only iterators and references to the *erased* elements are
  invalidated — everything else in the set stays valid. This holds
  because `set` is node-based; nodes never move, so erasing one
  element can't disturb pointers into another.
- The iterator overloads never throw. The key overloads may propagate
  an exception from `Compare`.

### Gotchas

- The classic erase-while-iterating bug: after `s.erase(it)`, `it`
  itself is invalidated, so `++it` on the old value is undefined
  behavior. Always reassign: `it = s.erase(it);` and skip the `++it`
  on that branch — every other iterator in the loop stays valid
  across the call.
- `erase(key)` on a missing key is a harmless no-op that returns `0`;
  it does not throw.
- `pos` must be dereferenceable — passing `end()` is undefined
  behavior, not a safe "erase nothing".

### Example

```cpp
#include <iostream>
#include <set>

int main()
{
    std::set<int> c = {1, 2, 3, 4};

    for (auto it = c.begin(); it != c.end();)
    {
        if (*it % 2 != 0)
            it = c.erase(it);   // reassign: it is now the next element
        else
            ++it;
    }

    std::cout << "erase 1, erased count: " << c.erase(1) << '\n';
    std::cout << "erase 2, erased count: " << c.erase(2) << '\n';
    std::cout << "erase 2, erased count: " << c.erase(2) << '\n';

    for (int n : c)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

```text
erase 1, erased count: 0
erase 2, erased count: 1
erase 2, erased count: 0
4 
```

### Reference

```cpp skip
iterator erase( iterator pos );  // (until C++23)
iterator erase( iterator pos )
    requires(!std::same_as<iterator, const_iterator>);  // (since C++23)
iterator erase( const_iterator pos );  // (since C++11)
iterator erase( iterator first, iterator last );  // (until C++11)
iterator erase( const_iterator first, const_iterator last );  // (since C++11)
size_type erase( const Key& key );
template< class K >
size_type erase( K&& x );  // (since C++23)
```

The C++23 `pos` overload is constrained so exactly one participates
when `iterator` and `const_iterator` are the same type. The `K&&`
overload participates only if `Compare::is_transparent` is valid and
neither `iterator` nor `const_iterator` is implicitly constructible
from `K`. Two earlier defects (LWG 130, LWG 2059) are folded into the
signatures above: the return type is `iterator`, not `void`, and both
the `iterator` and `const_iterator` `pos` overloads exist without
ambiguity.

### See also

- **clear** — removes all elements
- **find** — locates an element by key
- **extract** — removes an element without destroying it (C++17)

---
*Source: https://en.cppreference.com/w/cpp/container/set/erase*
