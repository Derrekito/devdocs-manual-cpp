# std::rotate

Left-rotates a range in place: the element at `middle` becomes the new
first element, and everything before it wraps around to the end. It's
the standard building block for "move this element/block to a new
position without erasing and re-inserting."

```cpp skip
std::rotate(first, middle, last);          // left rotation, in place
std::rotate(policy, first, middle, last);  // parallel        (since C++17)
```

Since C++20 the non-policy form is `constexpr`.

### What you provide

- **first, last** — forward iterators bounding the range to rotate.
- **middle** — the element that should end up first;
  `[first, middle)` and `[middle, last)` must both be valid
  sub-ranges, or behavior is undefined.
- **policy** — an execution policy such as `std::execution::par`
  (C++17) to run in parallel.

### Guarantees and costs

- Linear in `distance(first, last)`.
- Returns an iterator to the new location of the old `first` element:
  `last` if `first == middle`, `first` if `middle == last`, otherwise
  the position `first` moved to.
- Elements keep their relative order within each of the two swapped
  blocks; only the blocks' positions change.
- Runs faster with bidirectional or (better) random-access iterators
  than with plain forward iterators; some implementations vectorize it
  for contiguous iterators with trivially-swappable elements.
- Parallel overload: a throwing operation calls `std::terminate`;
  allocation failure throws `std::bad_alloc`.

### Gotchas

- `middle` outside `[first, last]` is undefined behavior — there's no
  bounds check.
- Left-rotate by `k` uses `middle = first + k`; right-rotate by `k`
  uses `middle = last - k`.
- It mutates in place. Use `std::rotate_copy` to write the rotated
  result elsewhere and keep the original untouched.
- `first == middle` or `middle == last` (including an empty range) is
  a well-defined no-op — no need to special-case it yourself.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v{1, 2, 3, 4, 5};
    std::rotate(v.begin(), v.begin() + 2, v.end());

    for (int x : v)
        std::cout << x << ' ';
    std::cout << '\n';
}
```

```text
3 4 5 1 2
```

### Reference

Full declarations:

```cpp skip
template< class ForwardIt >
ForwardIt rotate( ForwardIt first,
                  ForwardIt middle, ForwardIt last );  // (until C++20)
template< class ForwardIt >
constexpr ForwardIt rotate( ForwardIt first,
                            ForwardIt middle, ForwardIt last );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt >
ForwardIt rotate( ExecutionPolicy&& policy,
                  ForwardIt first,
                  ForwardIt middle, ForwardIt last );  // (2) (since C++17)
```

`ForwardIt` must meet ValueSwappable and LegacyForwardIterator; its
dereferenced type must be MoveAssignable and MoveConstructible. The
returned position is conceptually `first + (last - middle)`, though
`+`/`-` need not be supported by the iterator — that's just how the
position is described. A defect report (LWG 488, applied to C++98)
added the requirement that the new location of `first` be returned at
all; earlier wording left the return value unspecified.

### See also

- **rotate_copy** — copies the rotated range into a new destination
- **ranges::rotate (C++20)** — constrained version taking a range

---
*Source: https://en.cppreference.com/w/cpp/algorithm/rotate*
