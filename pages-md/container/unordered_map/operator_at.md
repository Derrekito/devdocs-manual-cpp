# std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::operator[]

Returns a reference to the mapped value for `key` — and if `key`
isn't present, **inserts it first**, with the mapped value
default-constructed (value-initialized). This is the headline
surprise, and it's exactly the same contract as `map::operator[]`:
merely reading `m[key]` for a missing key silently mutates the map.
Because it can insert, `operator[]` is non-const and cannot be called
on a `const unordered_map` — use `at()` or `find()` there.

```cpp skip
m[key] = value;   // insert-or-update: writes value either way
m[key];             // reads; also inserts a default T{} if key was absent
```

### What you provide

- **key** — an lvalue or rvalue key. The rvalue overload moves from
  `key` only when an insertion actually happens.

### Guarantees and costs

- Average O(1); worst case linear in the size of the container.
- If the resulting element count exceeds
  `max_load_factor() * bucket_count()`, the container rehashes. A
  rehash invalidates **all** iterators; without one, iterators are
  left alone.
- If an exception is thrown during the insertion, it has no effect.
- Requires `T` to be default-constructible — `operator[]` cannot be
  used on a map whose mapped type has no default constructor.

### Gotchas

- Simply reading `m[key]` for a key that isn't there inserts a
  default-constructed value — a lookup that looks read-only isn't.
  This is exploited deliberately by the counting idiom
  `++counts[word]` (each first sighting starts at value-initialized
  `0`), but it's an easy way to accidentally bloat a map when you only
  meant to check something.
- Won't compile on a `const unordered_map&` — reach for `at()` (throws
  on miss) or `find()` (returns `end()` on miss) there instead.
- Any iterator you're holding may be invalidated by the insertion if
  it triggers a rehash — don't assume an iterator survives a
  `operator[]` call the way it would on a `map`.
- `insert_or_assign` (C++17) returns whether an insertion happened and
  doesn't require default-constructibility of the mapped type;
  `try_emplace` (C++17) likewise avoids default-constructing `T` only
  to overwrite it immediately.

### Example

```cpp
#include <iostream>
#include <string>
#include <unordered_map>

int main()
{
    std::unordered_map<std::string, int> counts;

    for (const auto& w : {"a", "b", "a", "c", "a", "b"})
        ++counts[w];          // missing key -> inserted as 0, then incremented

    counts["d"];               // read-only looking, but still inserts {"d", 0}

    std::cout << "a:" << counts["a"] << " b:" << counts["b"]
               << " c:" << counts["c"] << " d:" << counts["d"] << '\n';
}
```

```text
a:3 b:2 c:1 d:0
```

### Reference

```cpp skip
T& operator[]( const Key& key );  // (since C++11)
T& operator[]( Key&& key );  // (since C++11)
```

Equivalent to `try_emplace(key).first->second` or
`try_emplace(std::move(key)).first->second` (since C++17): a
`value_type` is emplaced in place from the key and a value-initialized
mapped value only when the key is absent. Complexity: average O(1),
worst case linear in `size()`.

### See also

- **at** — bounds-checked access; throws instead of inserting
- **insert_or_assign** — inserts or assigns, returns success (C++17)
- **try_emplace** — inserts in place only if the key is missing (C++17)
- **find** — returns an iterator instead of inserting

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_map/operator_at*
