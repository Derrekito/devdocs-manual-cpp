# Standard library header <algorithm>

`<algorithm>` is part of the algorithm library: it's where nearly
every operation that scans, rearranges, searches, sorts, merges, or
compares a range of elements lives — `sort`, `find`, `count`,
`transform`, `reverse`, `unique`, and dozens more. Include it whenever
you're about to write a hand-rolled loop over a container; there's a
good chance the loop already exists here. (Numeric reductions like
`accumulate` live in `<numeric>` instead, not here.)

Every algorithm below takes an **iterator range**, not a container —
`std::sort(v.begin(), v.end())`, not `std::sort(v)`. Since C++20, most
also have a `std::ranges::` counterpart (see below) that *does* take
the container or range directly.

### Includes

- **`<initializer_list>`** (C++11) — `std::initializer_list`

### Non-modifying sequence operations

- **`all_of` / `any_of` / `none_of`** (C++11) — does a predicate hold
  for all, any, or none of the elements?
- **`for_each`** — apply a function to every element
- **`for_each_n`** (C++17) — apply a function to the first N elements
- **`count` / `count_if`** — count elements equal to a value / matching
  a predicate
- **`mismatch`** — first position where two ranges differ
- **`find` / `find_if` / `find_if_not`** (`find_if_not` is C++11) —
  first element equal to a value / matching / not matching a predicate
- **`find_end`** — last occurrence of a subsequence
- **`find_first_of`** — first element matching any of a set
- **`adjacent_find`** — first two adjacent equal (or predicate-matching)
  elements
- **`search`** — first occurrence of a subsequence
- **`search_n`** — first run of N consecutive equal elements

### Modifying sequence operations

- **`copy` / `copy_if`** (C++11 for `copy_if`) — copy a range to a new
  location, optionally filtered
- **`copy_n`** (C++11) — copy N elements
- **`copy_backward`** — copy in reverse iteration order (safe for
  overlapping ranges shifting rightward)
- **`move` / `move_backward`** (C++11) — same as `copy`/`copy_backward`,
  but moves instead of copies
- **`fill` / `fill_n`** — assign a value to every element / to N
  elements
- **`transform`** — apply a function, writing results to a destination
- **`generate` / `generate_n`** — assign the results of successive
  function calls
- **`remove` / `remove_if`** — shuffle non-matching elements to the
  front, return the new logical end (doesn't actually shrink the range
  — pair with `erase` on the container)
- **`remove_copy` / `remove_copy_if`** — copy, omitting matching
  elements
- **`replace` / `replace_if`** — overwrite matching elements with a new
  value
- **`replace_copy` / `replace_copy_if`** — copy, replacing matching
  elements
- **`swap` / `swap_ranges` / `iter_swap`** — exchange values, ranges of
  values, or the elements two iterators point to
- **`reverse` / `reverse_copy`** — reverse a range in place / into a
  copy
- **`rotate` / `rotate_copy`** — rotate a range left in place / into a
  copy
- **`shift_left` / `shift_right`** (C++20) — shift elements within a
  range, leaving a gap
- **`shuffle`** (C++11; replaced the now-removed `random_shuffle` in
  C++17) — randomly reorder elements
- **`sample`** (C++17) — pick N random elements from a sequence
- **`unique` / `unique_copy`** — collapse consecutive duplicates in
  place / into a copy

### Partitioning operations

- **`is_partitioned`** (C++11) — is the range already split by a
  predicate?
- **`partition`** — split a range into two groups by a predicate
- **`partition_copy`** (C++11) — split into two output ranges
- **`stable_partition`** — split, preserving each group's relative
  order
- **`partition_point`** (C++11) — find the boundary of an
  already-partitioned range

### Sorting operations

- **`is_sorted` / `is_sorted_until`** (C++11) — is the range sorted? /
  how much of it is?
- **`sort`** — sort the whole range
- **`partial_sort`** — sort only the first N elements
- **`partial_sort_copy`** — same, into a separate output range
- **`stable_sort`** — sort, preserving order among equal elements
- **`nth_element`** — put one element where it would land if sorted,
  partition around it, in O(N)

### Binary search operations (on sorted ranges)

- **`lower_bound` / `upper_bound`** — first element not less than / first
  element greater than a value
- **`binary_search`** — does a value exist in the range?
- **`equal_range`** — the sub-range of elements matching a value

### Other operations on sorted ranges

- **`merge`** — merge two sorted ranges into a new one
- **`inplace_merge`** — merge two consecutive sorted sub-ranges in
  place

### Set operations (on sorted ranges)

- **`includes`** — is one sorted range a subsequence of another?
- **`set_difference` / `set_intersection` / `set_symmetric_difference` /
  `set_union`** — the usual set algebra over two sorted ranges

### Heap operations

- **`is_heap` / `is_heap_until`** (C++11) — is the range a max-heap? /
  how much of it is?
- **`make_heap` / `push_heap` / `pop_heap`** — build a max-heap / add
  one element / remove the largest
- **`sort_heap`** — turn a max-heap into an ascending sorted range

### Minimum/maximum operations

- **`max` / `min`** — the greater / smaller of two values (or an
  `initializer_list`)
- **`max_element` / `min_element`** — iterator to the largest / smallest
  element in a range
- **`minmax`** (C++11) — both smaller and larger of two values
- **`minmax_element`** (C++11) — iterators to both smallest and largest
  in a range
- **`clamp`** (C++17) — constrain a value to `[lo, hi]`

### Comparison operations

- **`equal`** — do two ranges hold the same elements?
- **`lexicographical_compare`** — is one range lexicographically less
  than another?
- **`lexicographical_compare_three_way`** (C++20) — same, via `<=>`

### Permutation operations

- **`is_permutation`** (C++11) — is one range a reordering of another?
- **`next_permutation` / `prev_permutation`** — step to the next/previous
  lexicographic ordering of a range, in place

### Ranges algorithms (C++20)

Almost every algorithm listed above has a `std::ranges::` counterpart
of the same name (a niebloid), added in C++20: it takes a range (or an
iterator + sentinel pair) instead of an iterator pair, and accepts an
optional trailing projection. `std::ranges::sort(v)` instead of
`std::sort(v.begin(), v.end())`, for example.

A handful of ranges-side algorithms have no classic counterpart at
all — these are net-new, not reformulations of something above:

- **`ranges::find_last` / `find_last_if` / `find_last_if_not`** (C++23)
  — find the *last* matching element instead of the first
- **`ranges::contains` / `contains_subrange`** (C++23) — does the range
  contain this value / this subrange?
- **`ranges::starts_with` / `ends_with`** (C++23) — does one range start
  or end with another?
- **`ranges::fold_left` / `fold_left_first` / `fold_right` /
  `fold_right_last`** (C++23) — reduce a range to one value,
  left- or right-associative; `_with_iter` variants also return the
  ending iterator

One classic/ranges pair is version-skewed: the classic
`shift_left`/`shift_right` arrived in C++20, but their
`ranges::` counterparts didn't land until **C++23**.

### Return types (C++20)

A handful of small `ranges::*_result` struct templates bundle multiple
outputs together, since a niebloid can't return an out-parameter the
way an overloaded function taking a reference could:

- **`in_fun_result`** — an iterator and a function object
- **`in_in_result`** — two iterators (e.g. `mismatch`'s result)
- **`in_out_result`** — an iterator and an output iterator (e.g.
  `copy`'s result)
- **`in_in_out_result`**, **`in_out_out_result`** — three-way versions
  of the above
- **`min_max_result`** — two objects/references of the same type (e.g.
  `minmax`'s result)
- **`in_found_result`** — an iterator and a `bool` found-flag
- **`in_value_result`**, **`out_value_result`** (C++23) — an iterator
  and a value, for the fold algorithms

---
*Source: https://en.cppreference.com/w/cpp/header/algorithm*
