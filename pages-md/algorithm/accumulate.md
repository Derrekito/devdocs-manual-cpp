# std::accumulate

Folds a range into a single value, left to right, starting from
`init`. With no operation given it sums with `operator+`; with one
given it applies that instead (`op(acc, element)` each step). Lives in
`<numeric>`, not `<algorithm>`.

```cpp skip
std::accumulate(first, last, init);       // left fold with operator+
std::accumulate(first, last, init, op);   // left fold with op
```

Since C++20 both forms are `constexpr`, and the accumulator is moved
into `op` rather than copied on every step.

### What you provide

- **first, last** — input iterators bounding the range to fold.
- **init** — the starting value **and** the type of the result. This
  matters: `accumulate` never widens or promotes on its own.
- **op** — a callable taking the running accumulator and one element,
  returning the new accumulator. It must be assignable back into
  `init`'s type. It must not modify the range or invalidate its
  iterators — doing so is undefined behavior.

### Guarantees and costs

- One application of `op` (or `+`) per element, applied strictly in
  order — this is a genuine left fold, not an unordered reduction.
- The result type is `T`, exactly `init`'s type; every intermediate
  value is also `T` (elements get implicitly converted into it each
  step).
- No exception-safety surprises beyond what `op` itself can throw;
  there is no parallel overload (see `std::reduce` for that).

### Gotchas

- **The init value's type is the accumulator type.** Seeding a `double`
  sum with a bare `0` silently accumulates as `int`, truncating every
  partial sum — use `0.0`. See the example below.
- A sum that can overflow `int` needs a wider seed (`0L`, `0LL`), for
  the same reason.
- Because it's a strict left fold, order-dependent operations (string
  concatenation, non-commutative math) are safe here but would give a
  different answer under `std::reduce` — don't swap them without
  checking.

### Example

```cpp
#include <iostream>
#include <numeric>
#include <vector>

int main()
{
    std::vector<double> prices{1.50, 2.25, 3.75};

    double wrong = std::accumulate(prices.begin(), prices.end(), 0);
    double right = std::accumulate(prices.begin(), prices.end(), 0.0);

    std::cout << wrong << ' ' << right << '\n';
}
```

```text
6 7.5
```

### Reference

Full declarations:

```cpp skip
template< class InputIt, class T >
T accumulate( InputIt first, InputIt last, T init );  // (until C++20)
template< class InputIt, class T >
constexpr T accumulate( InputIt first, InputIt last, T init );  // (since C++20)
template< class InputIt, class T, class BinaryOperation >
T accumulate( InputIt first, InputIt last, T init,
              BinaryOperation op );  // (until C++20)
template< class InputIt, class T, class BinaryOperation >
constexpr T accumulate( InputIt first, InputIt last, T init,
                        BinaryOperation op );  // (since C++20)
```

`InputIt` must meet LegacyInputIterator; `T` must be CopyAssignable
and CopyConstructible. Formally, the accumulator `acc` starts at
`init` and updates as `acc = acc + *i` (until C++20) or
`acc = std::move(acc) + *i` (since C++20) — or with `op` in place of
`+` — for every iterator `i` in `[first, last)` in order; the return
value is `acc` after the last update. To right-fold, reverse the
argument order in the operation and pass reverse iterators. Two
defect reports apply: LWG 242 (C++98) clarified `op` must not have
side effects that modify the range; LWG 2055 / P0616R0 (C++20) made
`acc` move rather than copy on each step.

### See also

- **reduce** — like `accumulate` but unordered, so it can run in
  parallel (C++17)
- **transform_reduce** — fuses a transform and a reduce into one pass
  (C++17)
- **partial_sum** — like `accumulate` but keeps every intermediate sum
- **inner_product** — computes the inner product of two ranges
- **ranges::fold_left** — left-folds a range of elements (C++23)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/accumulate*
