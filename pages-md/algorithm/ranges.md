# Constrained algorithms (since C++20)

C++20 provides constrained versions of most algorithms in the namespace
`std::ranges`. In these algorithms, a range can be specified as either an
iterator-sentinel pair or as a single `range` argument, and projections and
pointer-to-member callables are supported. Additionally, the return types of
most algorithms have been changed to return all potentially useful information
computed during the execution of the algorithm.

### Constrained algorithms

**Non-modifying sequence operations**

- **ranges::all_ofranges::any_ofranges::none_of (C++20)(C++20)(C++20)** — checks
  if a predicate is `true` for all, any or none of the elements in a range
  (niebloid)
- **ranges::for_each (C++20)** — applies a function to a range of elements
  (niebloid)
- **ranges::for_each_n (C++20)** — applies a function object to the first N
  elements of a sequence (niebloid)
- **ranges::countranges::count_if (C++20)(C++20)** — returns the number of
  elements satisfying specific criteria (niebloid)
- **ranges::mismatch (C++20)** — finds the first position where two ranges
  differ (niebloid)
- **ranges::equal (C++20)** — determines if two sets of elements are the same
  (niebloid)
- **ranges::lexicographical_compare (C++20)** — returns `true` if one range is
  lexicographically less than another (niebloid)
- **ranges::findranges::find_ifranges::find_if_not (C++20)(C++20)(C++20)** —
  finds the first element satisfying specific criteria (niebloid)
- **ranges::find_lastranges::find_last_ifranges::find_last_if_not
  (C++23)(C++23)(C++23)** — finds the last element satisfying specific criteria
  (niebloid)
- **ranges::find_end (C++20)** — finds the last sequence of elements in a
  certain range (niebloid)
- **ranges::find_first_of (C++20)** — searches for any one of a set of elements
  (niebloid)
- **ranges::adjacent_find (C++20)** — finds the first two adjacent items that
  are equal (or satisfy a given predicate) (niebloid)
- **ranges::search (C++20)** — searches for a range of elements (niebloid)
- **ranges::search_n (C++20)** — searches for a number consecutive copies of an
  element in a range (niebloid)
- **ranges::containsranges::contains_subrange (C++23)(C++23)** — checks if the
  range contains the given element or subrange (niebloid)
- **ranges::starts_with (C++23)** — checks whether a range starts with another
  range (niebloid)
- **ranges::ends_with (C++23)** — checks whether a range ends with another range
  (niebloid)

**Modifying sequence operations**

- **ranges::copyranges::copy_if (C++20)(C++20)** — copies a range of elements to
  a new location (niebloid)
- **ranges::copy_n (C++20)** — copies a number of elements to a new location
  (niebloid)
- **ranges::copy_backward (C++20)** — copies a range of elements in backwards
  order (niebloid)
- **ranges::move (C++20)** — moves a range of elements to a new location
  (niebloid)
- **ranges::move_backward (C++20)** — moves a range of elements to a new
  location in backwards order (niebloid)
- **ranges::fill (C++20)** — assigns a range of elements a certain value
  (niebloid)
- **ranges::fill_n (C++20)** — assigns a value to a number of elements
  (niebloid)
- **ranges::transform (C++20)** — applies a function to a range of elements
  (niebloid)
- **ranges::generate (C++20)** — saves the result of a function in a range
  (niebloid)
- **ranges::generate_n (C++20)** — saves the result of N applications of a
  function (niebloid)
- **ranges::removeranges::remove_if (C++20)(C++20)** — removes elements
  satisfying specific criteria (niebloid)
- **ranges::remove_copyranges::remove_copy_if (C++20)(C++20)** — copies a range
  of elements omitting those that satisfy specific criteria (niebloid)
- **ranges::replaceranges::replace_if (C++20)(C++20)** — replaces all values
  satisfying specific criteria with another value (niebloid)
- **ranges::replace_copyranges::replace_copy_if (C++20)(C++20)** — copies a
  range, replacing elements satisfying specific criteria with another value
  (niebloid)
- **ranges::swap_ranges (C++20)** — swaps two ranges of elements (niebloid)
- **ranges::reverse (C++20)** — reverses the order of elements in a range
  (niebloid)
- **ranges::reverse_copy (C++20)** — creates a copy of a range that is reversed
  (niebloid)
- **ranges::rotate (C++20)** — rotates the order of elements in a range
  (niebloid)
- **ranges::rotate_copy (C++20)** — copies and rotate a range of elements
  (niebloid)
- **ranges::shuffle (C++20)** — randomly re-orders elements in a range
  (niebloid)
- **ranges::shift_leftranges::shift_right (C++23)** — shifts elements in a range
  (niebloid)
- **ranges::sample (C++20)** — selects N random elements from a sequence
  (niebloid)
- **ranges::unique (C++20)** — removes consecutive duplicate elements in a range
  (niebloid)
- **ranges::unique_copy (C++20)** — creates a copy of some range of elements
  that contains no consecutive duplicates (niebloid)

**Partitioning operations**

- **ranges::is_partitioned (C++20)** — determines if the range is partitioned by
  the given predicate (niebloid)
- **ranges::partition (C++20)** — divides a range of elements into two groups
  (niebloid)
- **ranges::partition_copy (C++20)** — copies a range dividing the elements into
  two groups (niebloid)
- **ranges::stable_partition (C++20)** — divides elements into two groups while
  preserving their relative order (niebloid)
- **ranges::partition_point (C++20)** — locates the partition point of a
  partitioned range (niebloid)

**Sorting operations**

- **ranges::is_sorted (C++20)** — checks whether a range is sorted into
  ascending order (niebloid)
- **ranges::is_sorted_until (C++20)** — finds the largest sorted subrange
  (niebloid)
- **ranges::sort (C++20)** — sorts a range into ascending order (niebloid)
- **ranges::partial_sort (C++20)** — sorts the first N elements of a range
  (niebloid)
- **ranges::partial_sort_copy (C++20)** — copies and partially sorts a range of
  elements (niebloid)
- **ranges::stable_sort (C++20)** — sorts a range of elements while preserving
  order between equal elements (niebloid)
- **ranges::nth_element (C++20)** — partially sorts the given range making sure
  that it is partitioned by the given element (niebloid)

**Binary search operations (on sorted ranges)**

- **ranges::lower_bound (C++20)** — returns an iterator to the first element
  *not less* than the given value (niebloid)
- **ranges::upper_bound (C++20)** — returns an iterator to the first element
  *greater* than a certain value (niebloid)
- **ranges::binary_search (C++20)** — determines if an element exists in a
  partially-ordered range (niebloid)
- **ranges::equal_range (C++20)** — returns range of elements matching a
  specific key (niebloid)

**Set operations (on sorted ranges)**

- **ranges::merge (C++20)** — merges two sorted ranges (niebloid)
- **ranges::inplace_merge (C++20)** — merges two ordered ranges in-place
  (niebloid)
- **ranges::includes (C++20)** — returns `true` if one sequence is a subsequence
  of another (niebloid)
- **ranges::set_difference (C++20)** — computes the difference between two sets
  (niebloid)
- **ranges::set_intersection (C++20)** — computes the intersection of two sets
  (niebloid)
- **ranges::set_symmetric_difference (C++20)** — computes the symmetric
  difference between two sets (niebloid)
- **ranges::set_union (C++20)** — computes the union of two sets (niebloid)

**Heap operations**

- **ranges::is_heap (C++20)** — checks if the given range is a max heap
  (niebloid)
- **ranges::is_heap_until (C++20)** — finds the largest subrange that is a max
  heap (niebloid)
- **ranges::make_heap (C++20)** — creates a max heap out of a range of elements
  (niebloid)
- **ranges::push_heap (C++20)** — adds an element to a max heap (niebloid)
- **ranges::pop_heap (C++20)** — removes the largest element from a max heap
  (niebloid)
- **ranges::sort_heap (C++20)** — turns a max heap into a range of elements
  sorted in ascending order (niebloid)

**Minimum/maximum operations**

- **ranges::max (C++20)** — returns the greater of the given values (niebloid)
- **ranges::max_element (C++20)** — returns the largest element in a range
  (niebloid)
- **ranges::min (C++20)** — returns the smaller of the given values (niebloid)
- **ranges::min_element (C++20)** — returns the smallest element in a range
  (niebloid)
- **ranges::minmax (C++20)** — returns the smaller and larger of two elements
  (niebloid)
- **ranges::minmax_element (C++20)** — returns the smallest and the largest
  elements in a range (niebloid)
- **ranges::clamp (C++20)** — clamps a value between a pair of boundary values
  (niebloid)

**Permutation operations**

- **ranges::is_permutation (C++20)** — determines if a sequence is a permutation
  of another sequence (niebloid)
- **ranges::next_permutation (C++20)** — generates the next greater
  lexicographic permutation of a range of elements (niebloid)
- **ranges::prev_permutation (C++20)** — generates the next smaller
  lexicographic permutation of a range of elements (niebloid)

### Constrained numeric operations

- **ranges::iota (C++23)** — fills a range with successive increments of the
  starting value (niebloid)

### Constrained fold operations

- **ranges::fold_left (C++23)** — left-folds a range of elements (niebloid)
- **ranges::fold_left_first (C++23)** — left-folds a range of elements using the
  first element as an initial value (niebloid)
- **ranges::fold_right (C++23)** — right-folds a range of elements (niebloid)
- **ranges::fold_right_last (C++23)** — right-folds a range of elements using
  the last element as an initial value (niebloid)
- **ranges::fold_left_with_iter (C++23)** — left-folds a range of elements, and
  returns a pair (iterator, value) (niebloid)
- **ranges::fold_left_first_with_iter (C++23)** — left-folds a range of elements
  using the first element as an initial value, and returns a pair (iterator,
  optional) (niebloid)

### Constrained uninitialized memory algorithms

- **ranges::uninitialized_copy (C++20)** — copies a range of objects to an
  uninitialized area of memory (niebloid)
- **ranges::uninitialized_copy_n (C++20)** — copies a number of objects to an
  uninitialized area of memory (niebloid)
- **ranges::uninitialized_fill (C++20)** — copies an object to an uninitialized
  area of memory, defined by a range (niebloid)
- **ranges::uninitialized_fill_n (C++20)** — copies an object to an
  uninitialized area of memory, defined by a start and a count (niebloid)
- **ranges::uninitialized_move (C++20)** — moves a range of objects to an
  uninitialized area of memory (niebloid)
- **ranges::uninitialized_move_n (C++20)** — moves a number of objects to an
  uninitialized area of memory (niebloid)
- **ranges::uninitialized_default_construct (C++20)** — constructs objects by
  default-initialization in an uninitialized area of memory, defined by a range
  (niebloid)
- **ranges::uninitialized_default_construct_n (C++20)** — constructs objects by
  default-initialization in an uninitialized area of memory, defined by a start
  and count (niebloid)
- **ranges::uninitialized_value_construct (C++20)** — constructs objects by
  value-initialization in an uninitialized area of memory, defined by a range
  (niebloid)
- **ranges::uninitialized_value_construct_n (C++20)** — constructs objects by
  value-initialization in an uninitialized area of memory, defined by a start
  and a count (niebloid)
- **ranges::destroy (C++20)** — destroys a range of objects (niebloid)
- **ranges::destroy_n (C++20)** — destroys a number of objects in a range
  (niebloid)
- **ranges::destroy_at (C++20)** — destroys an object at a given address
  (niebloid)
- **ranges::construct_at (C++20)** — creates an object at a given address
  (niebloid)

### Return types

- **ranges::in_fun_result (C++20)** — provides a way to store an iterator and a
  function object as a single unit (class template)
- **ranges::in_in_result (C++20)** — provides a way to store two iterators as a
  single unit (class template)
- **ranges::in_out_result (C++20)** — provides a way to store two iterators as a
  single unit (class template)
- **ranges::in_in_out_result (C++20)** — provides a way to store three iterators
  as a single unit (class template)
- **ranges::in_out_out_result (C++20)** — provides a way to store three
  iterators as a single unit (class template)
- **ranges::min_max_result (C++20)** — provides a way to store two objects or
  references of the same type as a single unit (class template)
- **ranges::in_found_result (C++20)** — provides a way to store an iterator and
  a boolean flag as a single unit (class template)
- **ranges::in_value_result (C++23)** — provides a way to store an iterator and
  a value as a single unit (class template)
- **ranges::out_value_result (C++23)** — provides a way to store an iterator and
  a value as a single unit (class template)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges` | 201911L | (C++20) | Ranges library and constrained
      algorithms
  `__cpp_lib_ranges_contains` | 202207L | (C++23) | `std::ranges::contains`
  `__cpp_lib_ranges_find_last` | 202207L | (C++23) | `std::ranges::find_last`
  `__cpp_lib_ranges_fold` | 202207L | (C++23) | `std::ranges` fold algorithms
  `__cpp_lib_ranges_iota` | 202202L | (C++23) | `std::ranges::iota`
  `__cpp_lib_ranges_starts_ends_with` | 202106L | (C++23) |
      `std::ranges::starts_with`, `std::ranges::ends_with`
  `__cpp_lib_shift` | 201806L | (C++20) | `std::shift_left` and
      `std::shift_right`
  202202L | (C++23) | `std::ranges::shift_left` and `std::ranges::shift_right`

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges*
