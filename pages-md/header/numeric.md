# Standard library header <numeric>

`<numeric>` is the numeric algorithms header — distinct from
`<algorithm>`, it's specifically for reducing or scanning a range of
*numbers* into a single value or a running sequence: sums, products,
inner products, prefix sums, and a little number theory (`gcd`/`lcm`).
Include it whenever you're folding a range down to one number, or
computing a running total across it.

`accumulate` folds strictly left-to-right, so it's safe with
non-associative operations. `reduce` and `transform_reduce` do the same
job but may combine elements out of order (and, with an execution
policy, in parallel) — use them only when the combining operation is
associative and commutative.

### Reductions

- **accumulate** — sums (or folds) a range into a single value,
  strictly left-to-right
- **reduce** (C++17) — like `accumulate`, but may combine elements out
  of order
- **transform_reduce** (C++17) — applies a function to each element (or
  pair of elements), then reduces the results out of order
- **inner_product** — computes the inner (dot) product of two ranges

### Prefix sums and scans

- **partial_sum** — writes the running sum of a range
- **inclusive_scan** (C++17) — like `partial_sum`, but may run out of
  order; the *i*-th output includes the *i*-th input
- **exclusive_scan** (C++17) — like `inclusive_scan`, but the *i*-th
  output excludes the *i*-th input
- **transform_inclusive_scan** / **transform_exclusive_scan** (C++17) —
  applies a function first, then computes the corresponding scan

### Differences and sequences

- **adjacent_difference** — the differences between each pair of
  adjacent elements
- **iota** (C++11) — fills a range with successive increments of a
  starting value
- **ranges::iota** (C++23) — niebloid counterpart of `iota`

### Number theory

- **gcd** / **lcm** (C++17) — greatest common divisor / least common
  multiple of two integers
- **midpoint** (C++20) — the midpoint between two numbers or pointers,
  without the overflow a naive `(a + b) / 2` risks

### Saturating arithmetic (C++26)

- **add_sat** / **sub_sat** / **mul_sat** / **div_sat** — addition,
  subtraction, multiplication, and division that clamp to the result
  type's range instead of overflowing
- **saturate_cast** — converts an integer to another integer type,
  clamping to the destination's range instead of overflowing

---
*Source: https://en.cppreference.com/w/cpp/header/numeric*
