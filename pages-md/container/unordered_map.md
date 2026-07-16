# std::unordered_map

A hash table mapping unique keys to values. Search, insertion, and
removal are average O(1) — the container hashes the key and jumps
straight to its bucket instead of walking a tree. Iteration order is
unspecified and can change on any insert that triggers a rehash; use
`std::map` when order matters.

A custom key type needs `std::hash<Key>` specialized (or a hasher
passed as a template argument) before it can be used as a key.

```cpp skip
std::unordered_map<std::string, int> m;
std::unordered_map<std::string, int> m{{"a", 1}, {"b", 2}};

m["key"] = value;                    // insert-or-overwrite, default-constructs
m.insert({"key", value});            // insert only if absent
m.insert_or_assign("key", value);    // insert-or-overwrite, no default (C++17)
m.try_emplace("key", value);         // insert-if-absent, no default    (C++17)
m.emplace("key", value);             // construct in place

m.find("key");                       // iterator, or end() if absent
m.contains("key");                   // bool                            (C++20)
m.count("key");                      // 0 or 1 (keys are unique)
m.erase("key");
m.reserve(n);                        // pre-size, avoids rehashing during growth
```

### Guarantees and costs

- `find`, `count`, `contains`, `insert`, `emplace`, `erase`: average
  O(1); **worst case O(n)** if many keys collide into the same bucket
  (e.g. a poor or adversarial hash function).
- `operator[]`: average O(1); default-constructs `T` and inserts it if
  the key is absent, then returns a reference — a write, not a plain
  read, and won't compile on a `const` map.
- Iterator invalidation:
  - Read-only operations, `swap`, `std::swap`: never invalidate
    anything (though `swap` does invalidate each container's `end()`
    marking the swap boundary).
  - `insert`, `emplace`, `emplace_hint`, `operator[]`: invalidate
    iterators only if the operation triggers a rehash; references and
    pointers to existing elements are never invalidated by these.
  - `erase`: invalidates only the iterator to the erased element.
  - `clear`, `rehash`, `reserve`, `operator=`: invalidate everything.
- References and pointers to stored keys/values are only invalidated
  by erasing that specific element — never by a rehash, even though a
  rehash does invalidate iterators.
- `reserve(n)` pre-sizes the bucket array for `n` elements, avoiding
  the rehashes a growing table would otherwise trigger.

### Gotchas

- Never rely on iteration order — it is unspecified and can reshuffle
  after any insert that causes a rehash.
- `operator[]` inserts a default-constructed value for a missing key,
  same trap as `std::map` — use `find`/`contains` for a read-only
  check, `insert_or_assign`/`try_emplace` (C++17) to avoid the
  implicit default-construct.
- Iterators do not survive a rehash even though pointers/references to
  elements do — don't hold an iterator across an `insert` that might
  grow the table.
- Need ordered keys? Use `std::map`. Need duplicate keys?
  `std::unordered_multimap`.

### Example

```cpp c++20
#include <iostream>
#include <string>
#include <unordered_map>

int main()
{
    std::unordered_map<std::string, std::string> u{
        {"RED", "#FF0000"}, {"GREEN", "#00FF00"}, {"BLUE", "#0000FF"}
    };

    u["BLACK"] = "#000000";

    std::cout << "RED is " << u["RED"] << '\n';
    std::cout << "has GREEN: " << std::boolalpha << u.contains("GREEN") << '\n';
}
```

```text
RED is #FF0000
has GREEN: true
```

### Reference

```cpp skip
template<
    class Key,
    class T,
    class Hash = std::hash<Key>,
    class KeyEqual = std::equal_to<Key>,
    class Allocator = std::allocator<std::pair<const Key, T>>
> class unordered_map;  // (1) (since C++11)
namespace pmr {
    template< class Key, class T, class Hash = std::hash<Key>,
              class KeyEqual = std::equal_to<Key> >
    using unordered_map = std::unordered_map<Key, T, Hash, KeyEqual,
        std::pmr::polymorphic_allocator<std::pair<const Key, T>>>;
}  // (2) (since C++17)
```

Elements are grouped into buckets by `Hash(key)`. Two keys are
equivalent if `KeyEqual` returns true for them; if two keys are
equivalent, `Hash` must return the same value for both. `value_type` is
`std::pair<const Key, T>`; `iterator`/`const_iterator` are
LegacyForwardIterator; `local_iterator`/`const_local_iterator` walk a
single bucket only. `unordered_map` meets the requirements of
Container, AllocatorAwareContainer, and UnorderedAssociativeContainer.

**Member functions**, grouped as upstream groups them:

- (constructor), (destructor), `operator=`, `get_allocator`

  Iterators
  - `begin`/`cbegin`, `end`/`cend`

  Capacity
  - `empty`, `size`, `max_size`

  Modifiers
  - `clear`
  - `insert`, `insert_range` (C++23)
  - `insert_or_assign` (C++17)
  - `emplace`, `emplace_hint`
  - `try_emplace` (C++17)
  - `erase`
  - `swap`
  - `extract`, `merge` (C++17) — move nodes out of / between containers
    without copying keys/values

  Lookup
  - `at` — bounds-checked access, throws `std::out_of_range`
  - `operator[]` — access or insert-default
  - `count`, `find`, `contains` (C++20), `equal_range`

  Bucket interface
  - `begin(n)`/`cbegin(n)`, `end(n)`/`cend(n)` — iterate one bucket
  - `bucket_count`, `max_bucket_count`, `bucket_size`, `bucket`

  Hash policy
  - `load_factor`, `max_load_factor`
  - `rehash` — set bucket count directly
  - `reserve` — set bucket count for at least n elements

  Observers
  - `hash_function`, `key_eq`

Non-member: `operator==`/`operator!=` (C++11; `operator!=` removed in
C++20 since `==` implies it), `std::swap` (C++11), `std::erase_if`
(C++20). A `pmr::unordered_map` alias and deduction guides (C++17) are
also provided.

### See also

- **map** — same key-value semantics, sorted by key, O(log n) instead
  of average O(1)
- **unordered_multimap** — same, but allows duplicate keys

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_map*
