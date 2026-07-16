# std::mismatch

Answers: *where do two ranges first differ?* Compares two ranges
element by element (with `operator==` or a predicate) and returns a pair
of iterators pointing at the first pair that doesn't match — one
position into each range.

```cpp skip
std::mismatch(first1, last1, first2);                 // range2 assumed >= range1's length
std::mismatch(first1, last1, first2, last2);           // both ends given   (since C++14)
std::mismatch(first1, last1, first2, pred);            // custom equality
std::mismatch(first1, last1, first2, last2, pred);     // both ends + pred  (since C++14)
std::mismatch(policy, first1, last1, first2, ...);      // parallel forms   (since C++17)
```

Since C++20 the non-policy overloads are `constexpr`.

### What you provide

- **first1, last1** — the first range.
- **first2** (**, last2**) — the start of the second range, and
  optionally its end. If `last2` is omitted, `first2 + (last1 - first1)`
  is used as the implied end — the second range must actually be at
  least that long, or behavior is undefined.
- **p** — a binary predicate: return `true` if two elements should count
  as equal. It must not modify its arguments and must accept both
  element types without requiring a non-const reference.
- **policy** — an execution policy (C++17); requires ForwardIt on both
  ranges instead of InputIt.

### Guarantees and costs

- Returns a `std::pair` of iterators to the first non-equal elements, one
  from each range.
- If no mismatch is found: with only `last1` given, the pair holds
  `last1` and the corresponding position in range2 — undefined behavior
  if range2 is actually shorter than range1 *(until C++14)*. With `last2`
  given explicitly, the pair holds whichever end (`last1` or `last2`) the
  comparison reaches first *(since C++14)*.
- Complexity: at most `last1 - first1` comparisons with only `last1`
  given; at most `min(last1 - first1, last2 - first2)` with both ends
  given.
- Parallel overloads: a predicate that throws calls `std::terminate` for
  standard policies; allocation failure throws `std::bad_alloc`.

### Gotchas

- Without an explicit `last2`, `mismatch` trusts range2 to be at least as
  long as range1 — if it might be shorter, pass `last2` (available since
  C++14) instead of relying on the implied-end overloads.
- A returned pair that equals both ranges' ends can mean either "the
  ranges are equal" or "one range ran out first" when only one end was
  supplied — compare each returned iterator against its own range's end
  to tell which.

### Example

This finds the longest prefix of a string that also appears, reversed,
at its end (possibly overlapping):

```cpp
#include <algorithm>
#include <iostream>
#include <string>

std::string mirror_ends(const std::string& in)
{
    return std::string(in.begin(),
                       std::mismatch(in.begin(), in.end(), in.rbegin()).first);
}

int main()
{
    std::cout << mirror_ends("abXYZba") << '\n'
              << mirror_ends("abca") << '\n'
              << mirror_ends("aba") << '\n';
}
```

```text
ab
a
aba
```

### Reference

Full declarations:

```cpp skip
template< class InputIt1, class InputIt2 >
std::pair<InputIt1, InputIt2>
    mismatch( InputIt1 first1, InputIt1 last1,
              InputIt2 first2 );  // (until C++20)
template< class InputIt1, class InputIt2 >
constexpr std::pair<InputIt1, InputIt2>
              mismatch( InputIt1 first1, InputIt1 last1,
                        InputIt2 first2 );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2 >
std::pair<ForwardIt1, ForwardIt2>
    mismatch( ExecutionPolicy&& policy, ForwardIt1 first1, ForwardIt1 last1,
              ForwardIt2 first2 );  // (2) (since C++17)
template< class InputIt1, class InputIt2, class BinaryPredicate >
std::pair<InputIt1, InputIt2>
    mismatch( InputIt1 first1, InputIt1 last1,
              InputIt2 first2,
              BinaryPredicate p );  // (until C++20)
template< class InputIt1, class InputIt2, class BinaryPredicate >
constexpr std::pair<InputIt1, InputIt2>
              mismatch( InputIt1 first1, InputIt1 last1,
                        InputIt2 first2,
                        BinaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
          class BinaryPredicate >
std::pair<ForwardIt1, ForwardIt2>
    mismatch( ExecutionPolicy&& policy, ForwardIt1 first1, ForwardIt1 last1,
              ForwardIt2 first2,
              BinaryPredicate p );  // (4) (since C++17)
template< class InputIt1, class InputIt2 >
std::pair<InputIt1, InputIt2>
    mismatch( InputIt1 first1, InputIt1 last1,
              InputIt2 first2, InputIt2 last2 );  // (since C++14) (until C++20)
template< class InputIt1, class InputIt2 >
constexpr std::pair<InputIt1, InputIt2>
              mismatch( InputIt1 first1, InputIt1 last1,
                        InputIt2 first2, InputIt2 last2 );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2 >
std::pair<ForwardIt1, ForwardIt2>
    mismatch( ExecutionPolicy&& policy, ForwardIt1 first1, ForwardIt1 last1,
              ForwardIt2 first2, ForwardIt2 last2 );  // (6) (since C++17)
template< class InputIt1, class InputIt2, class BinaryPredicate >
std::pair<InputIt1, InputIt2>
    mismatch( InputIt1 first1, InputIt1 last1,
              InputIt2 first2, InputIt2 last2,
              BinaryPredicate p );  // (since C++14) (until C++20)
template< class InputIt1, class InputIt2, class BinaryPredicate >
constexpr std::pair<InputIt1, InputIt2>
              mismatch( InputIt1 first1, InputIt1 last1,
                        InputIt2 first2, InputIt2 last2,
                        BinaryPredicate p );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
          class BinaryPredicate >
std::pair<ForwardIt1, ForwardIt2>
    mismatch( ExecutionPolicy&& policy, ForwardIt1 first1, ForwardIt1 last1,
              ForwardIt2 first2, ForwardIt2 last2,
              BinaryPredicate p );  // (8) (since C++17)
```

`InputIt1`/`InputIt2` must meet LegacyInputIterator; the policy overloads
require `ForwardIt1`/`ForwardIt2` (LegacyForwardIterator).
`BinaryPredicate` must meet the BinaryPredicate requirements.

### See also

- **equal** — determines if two ranges are the same
- **find**, **find_if** (C++11) — find the first element matching a
  criterion
- **lexicographical_compare** — orders one range against another
- **search** — searches for a subrange
- **ranges::mismatch** (C++20) — constrained version with projections

---
*Source: https://en.cppreference.com/w/cpp/algorithm/mismatch*
