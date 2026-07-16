# C++ named requirements: Compare

**Compare** is a set of requirements expected by some of the standard library
facilities from the user-provided function object types.

The return value of the function call operation applied to an object of a type
satisfying Compare, when contextually converted to `bool`, yields `true` if the
first argument of the call appears before the second in the strict weak ordering
relation induced by this type, and `false` otherwise.

As with any BinaryPredicate, evaluation of that expression is not allowed to
call non-const functions through the dereferenced iterators and, syntactically,
the function call operation must accept `const` object arguments, with the same
behavior regardless of whether the arguments are `const` or non-`const`(since
C++20).

### Requirements

The type `T` satisfies Compare if

- The type `T` satisfies BinaryPredicate, and

Given

- `comp`, an object of type `T`,
- `equiv(a, b)`, an expression-equivalent to `!comp(a, b) && !comp(b, a)`.

The following expressions must be valid and have their specified effects:

  Expression | Return type | Requirements
  `comp(a, b)` | implicitly convertible to `bool` | Establishes strict weak
      ordering relation with the following properties: For all `a`, `comp(a, a)
      == false`. If `comp(a, b) == true` then `comp(b, a) == false`. if `comp(a,
      b) == true` and `comp(b, c) == true` then `comp(a, c) == true`.
  `equiv(a, b)` | `bool` | Establishes equivalence relationship with the
      following properties: For all `a`, `equiv(a, a) == true`. If `equiv(a, b)
      == true`, then `equiv(b, a) == true`. If `equiv(a, b) == true` and
      `equiv(b, c) == true`, then `equiv(a, c) == true`.

Note: `comp` induces a strict total ordering on the equivalence classes
determined by `equiv`.

### Standard library

The following standard library facilities expect a Compare type.

- **set** — collection of unique keys, sorted by keys (class template)
- **map** — collection of key-value pairs, sorted by keys, keys are unique
  (class template)
- **multiset** — collection of keys, sorted by keys (class template)
- **multimap** — collection of key-value pairs, sorted by keys (class template)
- **priority_queue** — adapts a container to provide priority queue (class
  template)
- **sort** — sorts a range into ascending order (function template)
- **sort** — sorts the elements (public member function of
  `std::forward_list<T,Allocator>`)
- **sort** — sorts the elements (public member function of
  `std::list<T,Allocator>`)
- **stable_sort** — sorts a range of elements while preserving order between
  equal elements (function template)
- **partial_sort** — sorts the first N elements of a range (function template)
- **partial_sort_copy** — copies and partially sorts a range of elements
  (function template)
- **is_sorted (C++11)** — checks whether a range is sorted into ascending order
  (function template)
- **is_sorted_until (C++11)** — finds the largest sorted subrange (function
  template)
- **nth_element** — partially sorts the given range making sure that it is
  partitioned by the given element (function template)
- **lower_bound** — returns an iterator to the first element *not less* than the
  given value (function template)
- **upper_bound** — returns an iterator to the first element *greater* than a
  certain value (function template)
- **binary_search** — determines if an element exists in a partially-ordered
  range (function template)
- **equal_range** — returns range of elements matching a specific key (function
  template)
- **merge** — merges two sorted ranges (function template)
- **merge** — merges two sorted lists (public member function of
  `std::forward_list<T,Allocator>`)
- **merge** — merges two sorted lists (public member function of
  `std::list<T,Allocator>`)
- **inplace_merge** — merges two ordered ranges in-place (function template)
- **includes** — returns `true` if one sequence is a subsequence of another
  (function template)
- **set_difference** — computes the difference between two sets (function
  template)
- **set_intersection** — computes the intersection of two sets (function
  template)
- **set_symmetric_difference** — computes the symmetric difference between two
  sets (function template)
- **set_union** — computes the union of two sets (function template)
- **push_heap** — adds an element to a max heap (function template)
- **pop_heap** — removes the largest element from a max heap (function template)
- **make_heap** — creates a max heap out of a range of elements (function
  template)
- **sort_heap** — turns a max heap into a range of elements sorted in ascending
  order (function template)
- **is_heap (C++11)** — checks if the given range is a max heap (function
  template)
- **is_heap_until (C++11)** — finds the largest subrange that is a max heap
  (function template)
- **max** — returns the greater of the given values (function template)
- **max_element** — returns the largest element in a range (function template)
- **min** — returns the smaller of the given values (function template)
- **min_element** — returns the smallest element in a range (function template)
- **minmax (C++11)** — returns the smaller and larger of two elements (function
  template)
- **minmax_element (C++11)** — returns the smallest and the largest elements in
  a range (function template)
- **lexicographical_compare** — returns `true` if one range is lexicographically
  less than another (function template)
- **next_permutation** — generates the next greater lexicographic permutation of
  a range of elements (function template)
- **prev_permutation** — generates the next smaller lexicographic permutation of
  a range of elements (function template)

### See also

- **strict_weak_order (C++20)** — specifies that a `relation` imposes a strict
  weak ordering (concept)
- ****Comparison operators**** — `<`, `<=`, `>`, `>=`, `==`, `!=`, and `<=>`
  (C++20), compare the arguments

---
*Source: https://en.cppreference.com/w/cpp/named_req/Compare*
