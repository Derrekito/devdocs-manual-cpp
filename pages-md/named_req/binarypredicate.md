# C++ named requirements: BinaryPredicate

**BinaryPredicate** is a set of requirements expected by some of the standard
library facilities from the user-provided arguments.

Given a BinaryPredicate `bin_pred` and a pair of iterators `iter1` and `iter2`
or an iterator `iter` and a (possibly `const`)(since C++20) value `value`, the
expression `bin_pred(*iter1, *iter2)` or, respectively, `bin_pred(*iter,
value)`, must be contextually convertible to `bool`.

In addition, evaluation of that expression is not allowed to call non-const
member functions of the dereferenced iterators ; syntactically, the predicate
must accept `const` object arguments, with the same behavior regardless of
whether its arguments are `const` or non-`const`(since C++20).

### Requirements

- Predicate
- CopyConstructible (unless otherwise specified)

### Standard library

The following standard library facilities expect a BinaryPredicate which isn't a
Compare type.

- **unique** — removes consecutive duplicate elements (public member function of
  `std::forward_list<T,Allocator>`)
- **unique** — removes consecutive duplicate elements (public member function of
  `std::list<T,Allocator>`)
- **find_end** — finds the last sequence of elements in a certain range
  (function template)
- **find_first_of** — searches for any one of a set of elements (function
  template)
- **adjacent_find** — finds the first two adjacent items that are equal (or
  satisfy a given predicate) (function template)
- **mismatch** — finds the first position where two ranges differ (function
  template)
- **equal** — determines if two sets of elements are the same (function
  template)
- **is_permutation (C++11)** — determines if a sequence is a permutation of
  another sequence (function template)
- **search** — searches for a range of elements (function template)
- **search_n** — searches a range for a number of consecutive copies of an
  element (function template)
- **unique** — removes consecutive duplicate elements in a range (function
  template)
- **unique_copy** — creates a copy of some range of elements that contains no
  consecutive duplicates (function template)
- **not2 (deprecated in C++17)(removed in C++20)** — constructs custom
  `std::binary_negate` object (function template)
- **unordered_set (C++11)** — collection of unique keys, hashed by keys (class
  template)
- **unordered_map (C++11)** — collection of key-value pairs, hashed by keys,
  keys are unique (class template)
- **unordered_multiset (C++11)** — collection of keys, hashed by keys (class
  template)
- **unordered_multimap (C++11)** — collection of key-value pairs, hashed by keys
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/named_req/BinaryPredicate*
