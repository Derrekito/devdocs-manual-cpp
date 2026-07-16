# std::reverse

Reverses a range in place by swapping elements from the outside in.

```cpp skip
std::reverse(first, last);          // in-place reversal
std::reverse(policy, first, last);  // parallel        (since C++17)
```

Since C++20 the non-policy form is `constexpr`.

### What you provide

- **first, last** — bidirectional iterators (a `vector`, `deque`,
  `list`, `array`, or `std::string` all qualify; a forward-only range
  or an input stream does not).
- **policy** — an execution policy such as `std::execution::par`
  (C++17) to run in parallel.

### Guarantees and costs

- Exactly `distance(first, last) / 2` swaps — behaves as if
  `iter_swap` were applied to each pair `first + i` and
  `(last - i) - 1`.
- Mutates the range in place; there is no return value.
- Some implementations vectorize it for contiguous iterators whose
  value type swaps without a non-trivial special member or ADL
  `swap`.
- Parallel overload: a throwing swap calls `std::terminate`;
  allocation failure throws `std::bad_alloc`.

### Gotchas

- Needs **bidirectional** iterators — a forward list works with
  `reverse` (it's bidirectional via `std::list`), but a true
  forward-only range or an input stream does not compile.
- It mutates in place. To read backwards without mutating, use
  `rbegin()`/`rend()`, or `std::reverse_copy` to write the reversal
  into a separate range.
- Reversing a `std::string` byte-wise corrupts multi-byte UTF-8;
  reverse by code point or grapheme if the text isn't plain ASCII.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v{1, 2, 3, 4, 5};
    std::reverse(v.begin(), v.end());

    for (int x : v)
        std::cout << x << ' ';
    std::cout << '\n';
}
```

```text
5 4 3 2 1
```

### Reference

Full declarations:

```cpp skip
template< class BidirIt >
void reverse( BidirIt first, BidirIt last );  // (until C++20)
template< class BidirIt >
constexpr void reverse( BidirIt first, BidirIt last );  // (since C++20)
template< class ExecutionPolicy, class BidirIt >
void reverse( ExecutionPolicy&& policy,
              BidirIt first, BidirIt last );  // (2) (since C++17)
```

`BidirIt` must meet ValueSwappable and LegacyBidirectionalIterator.
Two defect reports (both applied to C++98) fixed early wording: LWG
223 corrected the spec to use `iter_swap` rather than `swap` on the
iterators themselves; LWG 2039 removed an extra swap that would have
fired when `i` equals `distance(first, last) / 2`.

### See also

- **reverse_copy** — writes a reversed copy to a separate range
- **ranges::reverse (C++20)** — constrained version taking a range

---
*Source: https://en.cppreference.com/w/cpp/algorithm/reverse*
