# operator==,!=(std::unordered_multiset)

```cpp
template< class Key, class Hash, class KeyEqual, class Alloc >
bool operator==( const std::unordered_multiset<Key, Hash, KeyEqual, Alloc>& lhs,
                 const std::unordered_multiset<Key, Hash, KeyEqual, Alloc>& rhs );  // (1)
template< class Key, class Hash, class KeyEqual, class Alloc >
bool operator!=( const std::unordered_multiset<Key, Hash, KeyEqual, Alloc>& lhs,
                 const std::unordered_multiset<Key, Hash, KeyEqual, Alloc>& rhs );  // (2) (until C++20)
```

Compares the contents of two unordered containers.

The contents of two unordered containers `lhs` and `rhs` are equal if the
following conditions hold:

- `lhs.size() == rhs.size()`.
- each group of equivalent elements `[``lhs_eq1``,``lhs_eq2``)` obtained from
  `lhs.equal_range(lhs_eq1)` has a corresponding group of equivalent elements in
  the other container `[``rhs_eq1``,``rhs_eq2``)` obtained from
  `rhs.equal_range(rhs_eq1)`, that has the following properties:

The behavior is undefined if `Key` is not EqualityComparable.

The behavior is also undefined if `hash_function()` and `key_eq()` do(until
C++20)`key_eq()` does(since C++20) not have the same behavior on `lhs` and `rhs`
or if `operator==` for `Key` is not a refinement of the partition into
equivalent-key groups introduced by `key_eq()` (that is, if two elements that
compare equal using `operator==` fall into different partitions).

The `!=` operator is synthesized from `operator==`.
*(since C++20)*

### Parameters

- **lhs, rhs** — unordered containers to compare

### Return value

1) `true` if the contents of the containers are equal, `false` otherwise.

2) `true` if the contents of the containers are not equal, `false` otherwise.

### Complexity

Proportional to *ΣSi2* calls to `operator==` on `value_type`, calls to the
predicate returned by `key_eq`, and calls to the hasher returned by
`hash_function` in the average case, where *S* is the size of the *i*th
equivalent key group. Proportional to *N2* in the worst case, where *N* is the
size of the container. Average case becomes proportional to *N* if the elements
within each equivalent key group are arranged in the same order (happens when
the containers are copies of each other).

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_multiset/operator_cmp*
