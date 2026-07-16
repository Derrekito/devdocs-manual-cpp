# std::set<Key,Compare,Allocator>::count

Returns how many elements compare equivalent to the given key — on a
`set` that's always **0 or 1**, since keys are unique (unlike
`multiset`, where it can be any count). If all you need is a yes/no
answer, `contains` (C++20) says the same thing more readably and
doesn't tempt you into treating the result as a real count.

```cpp skip
s.count(key);      // 0 or 1
s.count(x);         // heterogeneous lookup, needs transparent Compare (C++14)
```

### What you provide

- **key** — the key to count.
- **x** — a value of any type comparable to `Key` without converting
  it first. Requires `Compare::is_transparent` (C++14), same rule as
  `find`'s transparent overload.

### Guarantees and costs

- Logarithmic in the size of the container, plus linear in the number
  of elements found — on `set` that extra term is at most 1, so this
  is effectively O(log n).
- Returns `size_type`: 1 if an equivalent key is present, 0 otherwise.

### Gotchas

- `count` never returns anything but 0 or 1 on `set` — reaching for it
  as an existence check works, but reads oddly next to `contains`
  (C++20), which says exactly that and nothing more.
- On `multiset`, the same call can return any count and costs
  O(log n + count(key)) — don't port assumptions about `count`'s cost
  from `set` to `multiset` without accounting for that.

### Example

```cpp
#include <iostream>
#include <set>

int main()
{
    std::set<int> s{3, 1, 4, 1, 5};   // duplicates collapse: {1, 3, 4, 5}

    std::cout << s.count(1) << ", " << s.count(2) << '\n';
}
```

```text
1, 0
```

### Reference

```cpp skip
size_type count( const Key& key ) const;  // (1)
template< class K >
size_type count( const K& x ) const;  // (2) (since C++14)
```

Overload (2) participates in overload resolution only if
`Compare::is_transparent` is a valid type.

### See also

- **find** — returns an iterator instead of a count
- **contains** — plain existence check (C++20)
- **equal_range** — range of elements matching a key

---
*Source: https://en.cppreference.com/w/cpp/container/set/count*
