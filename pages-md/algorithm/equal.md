# std::equal

Compares two ranges element-by-element. The two-iterator form
`equal(first1, last1, first2)` trusts that the second range has at
least `last1 - first1` elements — it never checks `first2`'s bounds,
so a shorter second range is undefined behavior (a likely
out-of-bounds read). Prefer the four-iterator form
`equal(first1, last1, first2, last2)` (C++14) when lengths aren't
already guaranteed equal; it checks lengths first and returns `false`
on a mismatch without reading past either range. For comparing whole
containers of the same type, prefer the container's own `operator==`
over `std::equal` — it's clearer and, for containers that track their
own size, cheaper.

```cpp skip
std::equal(first1, last1, first2);               // trusts len(range2) >= len(range1)
std::equal(first1, last1, first2, pred);          // same, with a predicate
std::equal(first1, last1, first2, last2);         // checks lengths first (since C++14)
std::equal(first1, last1, first2, last2, pred);   // same, with a predicate (since C++14)
```

Policy-taking overloads of each form exist for parallel execution
(since C++17). `constexpr` since C++20.

### What you provide

- **first1, last1** — the first range (LegacyInputIterator).
- **first2** (, **last2**) — the second range; `last2` is required for
  the four-iterator overloads and optional for the two-iterator ones.
- **p** — a callable returning `true` if two elements should count as
  equal. Must not modify what it's given. Defaults to `operator==`.
- **policy** — an execution policy (C++17) for parallel execution;
  requires ForwardIt on both ranges, not just InputIt.

### Guarantees and costs

- Two-iterator form: at most `last1 - first1` predicate applications.
- Four-iterator form: at most `min(last1-first1, last2-first2)`
  applications — but if both iterator types are random-access and the
  lengths differ, it detects the mismatch and returns `false` without
  applying the predicate at all.
- Policy overloads: complexity is stated as exactly that many
  applications, not "at most."
- Parallel overloads: if the predicate throws, `std::terminate` is
  called; allocation failure throws `std::bad_alloc`.

### Gotchas

- The two-iterator overloads do not check that the second range is
  long enough — passing a shorter `range2` is UB, not a clean `false`.
  Use the four-iterator overload unless you already know the lengths
  match.
- Don't use `std::equal` on `std::unordered_set`/`unordered_map` (or
  their multi- variants): equal elements can be stored in different
  orders in two such containers, so a position-by-position compare
  can report `false` for containers that are logically the same.

### Example

```cpp
#include <algorithm>
#include <iomanip>
#include <iostream>
#include <string_view>

constexpr bool is_palindrome(const std::string_view& s)
{
    return std::equal(s.cbegin(), s.cbegin() + s.size() / 2, s.crbegin());
}

void test(const std::string_view& s)
{
    std::cout << std::quoted(s)
              << (is_palindrome(s) ? " is" : " is not")
              << " a palindrome\n";
}

int main()
{
    test("radar");
    test("hello");
}
```

```text
"radar" is a palindrome
"hello" is not a palindrome
```

### Reference

Full declarations:

```cpp skip
template< class InputIt1, class InputIt2 >
bool equal( InputIt1 first1, InputIt1 last1,
            InputIt2 first2 );  // (1) (until C++20)
template< class InputIt1, class InputIt2 >
constexpr bool equal( InputIt1 first1, InputIt1 last1,
                      InputIt2 first2 );  // (1) (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2 >
bool equal( ExecutionPolicy&& policy,
            ForwardIt1 first1, ForwardIt1 last1,
            ForwardIt2 first2 );  // (2) (since C++17)
template< class InputIt1, class InputIt2, class BinaryPredicate >
bool equal( InputIt1 first1, InputIt1 last1,
            InputIt2 first2, BinaryPredicate p );  // (3) (until C++20)
template< class InputIt1, class InputIt2, class BinaryPredicate >
constexpr bool equal( InputIt1 first1, InputIt1 last1,
                      InputIt2 first2, BinaryPredicate p );  // (3) (since C++20)
template< class ExecutionPolicy,
          class ForwardIt1, class ForwardIt2, class BinaryPredicate >
bool equal( ExecutionPolicy&& policy,
            ForwardIt1 first1, ForwardIt1 last1,
            ForwardIt2 first2, BinaryPredicate p );  // (4) (since C++17)
template< class InputIt1, class InputIt2 >
bool equal( InputIt1 first1, InputIt1 last1,
            InputIt2 first2, InputIt2 last2 );  // (5) (since C++14) (until C++20)
template< class InputIt1, class InputIt2 >
constexpr bool equal( InputIt1 first1, InputIt1 last1,
                      InputIt2 first2, InputIt2 last2 );  // (5) (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2 >
bool equal( ExecutionPolicy&& policy,
            ForwardIt1 first1, ForwardIt1 last1,
            ForwardIt2 first2, ForwardIt2 last2 );  // (6) (since C++17)
template< class InputIt1, class InputIt2, class BinaryPredicate >
bool equal( InputIt1 first1, InputIt1 last1,
            InputIt2 first2, InputIt2 last2,
            BinaryPredicate p );  // (7) (since C++14) (until C++20)
template< class InputIt1, class InputIt2, class BinaryPredicate >
constexpr bool equal( InputIt1 first1, InputIt1 last1,
                      InputIt2 first2, InputIt2 last2,
                      BinaryPredicate p );  // (7) (since C++20)
template< class ExecutionPolicy,
          class ForwardIt1, class ForwardIt2, class BinaryPredicate >
bool equal( ExecutionPolicy&& policy,
            ForwardIt1 first1, ForwardIt1 last1,
            ForwardIt2 first2, ForwardIt2 last2,
            BinaryPredicate p );  // (8) (since C++17)
```

`InputIt1`/`InputIt2` must meet LegacyInputIterator; the policy
overloads' `ForwardIt1`/`ForwardIt2` must meet LegacyForwardIterator.
Two ranges are equal if they have the same length and, for every `i`
in `[first1, last1)`, `*i` compares equal to the corresponding element
of the second range.

### See also

- **find**, **find_if**, **find_if_not** — locate the first matching element (C++11)
- **lexicographical_compare** — `true` if one range sorts before another
- **mismatch** — finds the first position where two ranges differ
- **search** — searches for a subrange within a range
- **equal_to** — function object implementing `x == y`
- **ranges::equal** — constrained version (C++20)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/equal*
