# std::unordered_set

A hash table of unique keys — the fast membership test. Search,
insertion, and removal are average O(1): the container hashes the
value and jumps straight to its bucket. Iteration order is unspecified
and can change on any insert that triggers a rehash; use `std::set`
when order matters.

A custom key type needs `std::hash<Key>` specialized (or a hasher
passed as a template argument) before it can be used as a key.

```cpp skip
std::unordered_set<int> s;
std::unordered_set<int> s{2, 7, 1, 8};

s.insert(x);                         // returns {iterator, bool inserted}
s.emplace(args...);                  // construct in place

s.find(x);                           // iterator, or end() if absent
s.contains(x);                       // bool                            (C++20)
s.count(x);                          // 0 or 1 (elements are unique)
s.erase(x);
s.reserve(n);                        // pre-size, avoids rehashing during growth
```

### Guarantees and costs

- `find`, `count`, `contains`, `insert`, `emplace`, `erase`: average
  O(1); **worst case O(n)** if many elements collide into the same
  bucket.
- Iterator invalidation:
  - Read-only operations, `swap`, `std::swap`: never invalidate
    anything (though `swap` invalidates each container's `end()`
    marking the swap boundary).
  - `insert`, `emplace`, `emplace_hint`: invalidate iterators only if
    the operation triggers a rehash.
  - `erase`: invalidates only the iterator to the erased element.
  - `clear`, `rehash`, `reserve`, `operator=`: invalidate everything.
- References and pointers to stored elements are only invalidated by
  erasing that specific element — never by a rehash, even though a
  rehash does invalidate iterators.
- After a move-assignment (unless incompatible allocators force an
  element-wise move), references, pointers, and non-end iterators into
  the moved-from container stay valid and now refer to elements in the
  target container.
- `reserve(n)` pre-sizes the bucket array for `n` elements, avoiding
  the rehashes a growing table would otherwise trigger.
- Elements cannot be modified in place, even through a non-const
  iterator — mutating a value could change its hash and corrupt the
  table.

### Gotchas

- Never rely on iteration order — it is unspecified and can reshuffle
  after any insert that causes a rehash.
- Use the member `s.count(x)` / `s.find(x)`, not the
  `std::find(s.begin(), s.end(), x)` algorithm — the member is average
  O(1), the generic algorithm is a linear O(n) scan.
- Iterators do not survive a rehash even though pointers/references to
  elements do — don't hold an iterator across an `insert` that might
  grow the table.
- Need ordered keys? Use `std::set`. Need duplicate keys?
  `std::unordered_multiset`.

### Example

```cpp c++20
#include <iostream>
#include <unordered_set>

int main()
{
    std::unordered_set<int> s{2, 7, 1, 8, 2, 8};   // duplicates dropped

    s.insert(5);
    std::cout << "has 5: " << std::boolalpha << s.contains(5) << '\n';
    std::cout << "size: " << s.size() << '\n';
}
```

```text
has 5: true
size: 5
```

### Reference

```cpp skip
template<
    class Key,
    class Hash = std::hash<Key>,
    class KeyEqual = std::equal_to<Key>,
    class Allocator = std::allocator<Key>
> class unordered_set;  // (1) (since C++11)
namespace pmr {
    template< class Key, class Hash = std::hash<Key>,
              class Pred = std::equal_to<Key> >
    using unordered_set = std::unordered_set<Key, Hash, Pred,
                               std::pmr::polymorphic_allocator<Key>>;
}  // (2) (since C++17)
```

Elements are grouped into buckets by `Hash(value)`. Two keys are
equivalent if `KeyEqual` returns true for them; if two keys are
equivalent, `Hash` must return the same value for both. `value_type` is
`Key`; `iterator` is a constant LegacyForwardIterator;
`local_iterator`/`const_local_iterator` walk a single bucket only.
`unordered_set` meets the requirements of Container,
AllocatorAwareContainer, and UnorderedAssociativeContainer. `iterator`
and `const_iterator` may alias the same type — prefer overloading on
`const_iterator` to avoid an accidental ODR violation.

**Member functions**, grouped as upstream groups them:

- (constructor), (destructor), `operator=`, `get_allocator`

  Iterators
  - `begin`/`cbegin`, `end`/`cend`

  Capacity
  - `empty`, `size`, `max_size`

  Modifiers
  - `clear`
  - `insert`, `insert_range` (C++23)
  - `emplace`, `emplace_hint`
  - `erase`
  - `swap`
  - `extract`, `merge` (C++17) — move nodes out of / between containers
    without copying keys

  Lookup
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
(C++20). A `pmr::unordered_set` alias and deduction guides (C++17) are
also provided.

### See also

- **set** — same uniqueness semantics, sorted by key, O(log n) instead
  of average O(1)
- **unordered_multiset** — same, but allows duplicate keys

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_set*
