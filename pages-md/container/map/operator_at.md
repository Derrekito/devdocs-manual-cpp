# std::map<Key,T,Compare,Allocator>::operator[]

Returns a reference to the mapped value for `key` — and if `key`
isn't present, **inserts it first**, with the mapped value
default-constructed (value-initialized). This is the headline
surprise: merely reading `m[key]` for a missing key silently mutates
the map. Because it can insert, `operator[]` is non-const and cannot
be called on a `const map` — use `at()` or `find()` there.

```cpp skip
m[key] = value;   // insert-or-update: writes value either way
m[key];             // reads; also inserts a default T{} if key was absent
```

### What you provide

- **key** — an lvalue or (C++11) rvalue key. The rvalue overload moves
  from `key` only when an insertion actually happens.

### Guarantees and costs

- O(log n), whether or not an insertion happens.
- No iterators or references are invalidated.
- If an exception is thrown during the insertion, it has no effect.
- Requires `T` to be default-constructible — `operator[]` cannot be
  used on a map whose mapped type has no default constructor.

### Gotchas

- Simply reading `m[key]` for a key that isn't there inserts a
  default-constructed value — a lookup that looks read-only isn't.
  This is exploited deliberately by the counting idiom
  `++counts[word]` (each first sighting starts at value-initialized
  `0`), but it's an easy way to accidentally bloat a map when you
  only meant to check something.
- Won't compile on a `const map&` — reach for `at()` (throws on miss)
  or `find()` (returns `end()` on miss) there instead.
- Since C++17, `insert_or_assign` returns whether an insertion
  happened and doesn't require `T` to be default-constructible;
  `try_emplace` (C++17) likewise avoids default-constructing `T` only
  to overwrite it immediately.

### Example

```cpp
#include <iostream>
#include <map>
#include <string>

int main()
{
    std::map<std::string, int> counts;

    for (const auto& w : {"a", "b", "a", "c", "a", "b"})
        ++counts[w];          // missing key -> inserted as 0, then incremented

    counts["d"];               // read-only looking, but still inserts {"d", 0}

    for (const auto& [word, n] : counts)
        std::cout << word << ':' << n << ' ';
    std::cout << '\n';
}
```

```text
a:3 b:2 c:1 d:0
```

### Reference

```cpp skip
T& operator[]( const Key& key );  // (1)
T& operator[]( Key&& key );  // (2) (since C++11)
```

Equivalent to `try_emplace(key).first->second` (1) or
`try_emplace(std::move(key)).first->second` (2) (since C++17): a
`value_type` is emplaced in place from the key and a value-initialized
mapped value only when the key is absent. Complexity: logarithmic in
the size of the container.

### See also

- **at** — bounds-checked access; throws instead of inserting
- **insert_or_assign** — inserts or assigns, returns success (C++17)
- **try_emplace** — inserts in place only if the key is missing (C++17)
- **find** — returns an iterator instead of inserting

---
*Source: https://en.cppreference.com/w/cpp/container/map/operator_at*
