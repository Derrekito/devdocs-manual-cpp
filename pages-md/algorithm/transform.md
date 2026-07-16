# std::transform

Applies a function to every element of a range (or to paired elements
of two ranges) and writes the results to a destination range,
preserving order. It does not resize the destination — that range must
already have room, or you must write through an inserting iterator
like `std::back_inserter`.

```cpp skip
std::transform(first, last, d_first, unary_op);            // one range in
std::transform(first1, last1, first2, d_first, binary_op);  // two ranges in
std::transform(policy, first, last, d_first, unary_op);     // parallel        (since C++17)
std::transform(policy, first1, last1, first2, d_first, binary_op);  // parallel (since C++17)
```

Since C++20 the non-policy forms are `constexpr`.

### What you provide

- **first, last** (and **first2** for the binary form) — input
  iterators bounding the source range(s); the second range is assumed
  to have at least as many elements as `[first1, last1)`.
- **d_first** — start of the destination range. May equal `first1` or
  `first2` to transform in place.
- **unary_op / binary_op** — a callable taking one element (unary) or
  one element from each range (binary), by value or const reference,
  and returning the value to store. It must not modify the elements it
  reads or invalidate any iterator in the ranges involved, including
  the end iterators.
- **policy** — an execution policy such as `std::execution::par`
  (C++17) to transform with multiple threads.

### Guarantees and costs

- Exactly `std::distance(first1, last1)` applications of the
  operation — never more, never fewer.
- Returns an output iterator to the element just past the last one
  written.
- Does **not** guarantee left-to-right application order, even in the
  non-parallel form — if the operation has side effects that must run
  in sequence, use `std::for_each` instead.
- Parallel overloads: if the operation throws, `std::terminate` is
  called; allocation failure throws `std::bad_alloc`.

### Gotchas

- Writing to a destination with no room is undefined behavior —
  `resize` it first, or use `std::back_inserter(out)`.
- Because order isn't guaranteed, don't rely on `transform` for
  sequenced side effects (a counter, printing) — that's what
  `for_each` promises and `transform` doesn't.
- `unary_op`/`binary_op` must leave the source elements alone; a
  callable that mutates through a reference parameter is out of
  contract even though it may appear to work.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v{1, 2, 3, 4};
    std::vector<int> out(v.size());

    std::transform(v.begin(), v.end(), out.begin(),
                   [](int x) { return x * x; });

    for (int x : out)
        std::cout << x << ' ';
    std::cout << '\n';
}
```

```text
1 4 9 16
```

### Reference

Full declarations:

```cpp skip
template< class InputIt, class OutputIt, class UnaryOperation >
OutputIt transform( InputIt first1, InputIt last1,
                    OutputIt d_first, UnaryOperation unary_op );  // (until C++20)
template< class InputIt, class OutputIt, class UnaryOperation >
constexpr OutputIt transform( InputIt first1, InputIt last1,
                              OutputIt d_first, UnaryOperation unary_op );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1,
          class ForwardIt2, class UnaryOperation >
ForwardIt2 transform( ExecutionPolicy&& policy,
                      ForwardIt1 first1, ForwardIt1 last1,
                      ForwardIt2 d_first, UnaryOperation unary_op );  // (since C++17)
template< class InputIt1, class InputIt2,
          class OutputIt, class BinaryOperation >
OutputIt transform( InputIt1 first1, InputIt1 last1, InputIt2 first2,
                    OutputIt d_first, BinaryOperation binary_op );  // (until C++20)
template< class InputIt1, class InputIt2,
          class OutputIt, class BinaryOperation >
constexpr OutputIt transform( InputIt1 first1, InputIt1 last1, InputIt2 first2,
                              OutputIt d_first, BinaryOperation binary_op );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
          class ForwardIt3, class BinaryOperation >
ForwardIt3 transform( ExecutionPolicy&& policy,
                      ForwardIt1 first1, ForwardIt1 last1, ForwardIt2 first2,
                      ForwardIt3 d_first, BinaryOperation binary_op );  // (since C++17)
```

`InputIt`/`InputIt1`/`InputIt2` must meet LegacyInputIterator,
`OutputIt` must meet LegacyOutputIterator, and the policy overloads'
`ForwardIt1`/`ForwardIt2`/`ForwardIt3` must meet LegacyForwardIterator.
The operation's parameter and return types just need to support the
implicit conversions to/from the iterators' value types — no exact
signature is required. A defect report (LWG 242, applied to C++98)
clarified that the operation must not have side effects that modify
the ranges involved, beyond returning a value.

### See also

- **for_each** — applies a function in guaranteed order, for side
  effects rather than a mapped result
- **ranges::transform** — constrained version with projections (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/transform*
