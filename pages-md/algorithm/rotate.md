# std::rotate

```cpp
template< class ForwardIt >
ForwardIt rotate( ForwardIt first, ForwardIt middle, ForwardIt last );  // (until C++20)
template< class ForwardIt >
constexpr ForwardIt rotate( ForwardIt first,
                            ForwardIt middle, ForwardIt last );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt >
ForwardIt rotate( ExecutionPolicy&& policy,
                  ForwardIt first, ForwardIt middle, ForwardIt last );  // (2) (since C++17)
```

1) Performs a left rotation on a range of elements.

Specifically, `std::rotate` swaps the elements in the range
   `[``first``,``last``)` in such a way that the elements in
   `[``first``,``middle``)` are placed after the elements in
   `[``middle``,``last``)` while the orders of the elements in both ranges are
   preserved.

2) Same as (1), but executed according to `policy`. This overload does not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

If `[``first``,``middle``)` or `[``middle``,``last``)` is not a valid range, the
behavior is undefined.

### Parameters

- **first** — the beginning of the original range
- **middle** — the element that should appear at the beginning of the rotated
  range
- **last** — the end of the original range
- **policy** — the execution policy to use. See execution policy for details.

**Type requirements**

**-`ForwardIt` must meet the requirements of ValueSwappable and LegacyForwardIterator.**

**-The type of dereferenced `ForwardIt` must meet the requirements of MoveAssignable and MoveConstructible.**

### Return value

An iterator that is equal to:

- `last`, if `first == middle` is `true`,
- `first`, if `middle == last` is `true`,
- `first + (last - middle)`[1] otherwise, i.e. the new location of the element
  pointed by `first`.

1. The `+` and `-` operations are not required to be supported, they are only
   used to represent to position of the returned iterator.

### Complexity

Linear in the distance between `first` and `last`.

### Exceptions

The overload with a template parameter named `ExecutionPolicy` reports errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

`std::rotate` has better efficiency on common implementations if `ForwardIt`
satisfies LegacyBidirectionalIterator or (better) LegacyRandomAccessIterator.

Implementations (e.g. MSVC STL) may enable vectorization when the iterator type
satisfies LegacyContiguousIterator and swapping its value type calls neither
non-trivial special member function nor ADL-found `swap`.

### Possible implementation

See also the implementations in libstdc++, libc++, and MSVC STL.

```cpp
template<class ForwardIt>
constexpr // since C++20
ForwardIt rotate(ForwardIt first, ForwardIt middle, ForwardIt last)
{
    if (first == middle)
        return last;

    if (middle == last)
        return first;

    ForwardIt write = first;
    ForwardIt next_read = first; // read position for when "read" hits "last"

    for (ForwardIt read = middle; read != last; ++write, ++read)
    {
        if (write == next_read)
            next_read = read; // track where "first" went
        std::iter_swap(write, read);
    }

    // rotate the remaining sequence into place
    rotate(write, next_read, last);
    return write;
}
```

### Example

`std::rotate` is a common building block in many algorithms. This example
demonstrates insertion sort.

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

auto print = [](auto const remark, auto const& v)
{
    std::cout << remark;
    for (auto n : v)
        std::cout << n << ' ';
    std::cout << '\n';
};

int main()
{
    std::vector<int> v {2, 4, 2, 0, 5, 10, 7, 3, 7, 1};
    print("before sort:\t\t", v);

    // insertion sort
    for (auto i = v.begin(); i != v.end(); ++i)
        std::rotate(std::upper_bound(v.begin(), i, *i), i, i + 1);
    print("after sort:\t\t", v);

    // simple rotation to the left
    std::rotate(v.begin(), v.begin() + 1, v.end());
    print("simple rotate left:\t", v);

    // simple rotation to the right
    std::rotate(v.rbegin(), v.rbegin() + 1, v.rend());
    print("simple rotate right:\t", v);
}
```

Output:

```text
before sort:                2 4 2 0 5 10 7 3 7 1
after sort:                0 1 2 2 3 4 5 7 7 10
simple rotate left:        1 2 2 3 4 5 7 7 10 0
simple rotate right:        0 1 2 2 3 4 5 7 7 10
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 488 | C++98 | the new location of the element pointed by `first` was not
      returned | returned

### See also

- **rotate_copy** — copies and rotate a range of elements (function template)
- **ranges::rotate (C++20)** — rotates the order of elements in a range
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/rotate*
