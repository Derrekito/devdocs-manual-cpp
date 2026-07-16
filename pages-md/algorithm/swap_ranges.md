# std::swap_ranges

```cpp
template< class ForwardIt1, class ForwardIt2 >
ForwardIt2 swap_ranges( ForwardIt1 first1, ForwardIt1 last1,
                        ForwardIt2 first2 );  // (until C++20)
template< class ForwardIt1, class ForwardIt2 >
constexpr ForwardIt2 swap_ranges( ForwardIt1 first1, ForwardIt1 last1,
                                  ForwardIt2 first2 );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2 >
ForwardIt2 swap_ranges( ExecutionPolicy&& policy,
                        ForwardIt1 first1, ForwardIt1 last1, ForwardIt2 first2 );  // (2) (since C++17)
```

1) Exchanges elements between range `[``first1``,``last1``)` and another range
   starting at `first2`.

*Precondition*: the two ranges `[``first1``,``last1``)` and
   `[``first2``,``last2``)` do not overlap, where `last2 = std::next(first2,
   std::distance(first1, last1))`.

2) Same as (1), but executed according to `policy`. This overload does not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first1, last1** — the first range of elements to swap
- **first2** — beginning of the second range of elements to swap
- **policy** — the execution policy to use. See execution policy for details.

**Type requirements**

**-`ForwardIt1, ForwardIt2` must meet the requirements of LegacyForwardIterator.**

**-The types of dereferenced `ForwardIt1` and `ForwardIt2` must meet the requirements of Swappable.**

### Return value

Iterator to the element past the last element exchanged in the range beginning
with `first2`.

### Complexity

Linear in the distance between `first1` and `last1`.

### Exceptions

The overload with a template parameter named `ExecutionPolicy` reports errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

Implementations (e.g. MSVC STL) may enable vectorization when the iterator type
satisfies LegacyContiguousIterator and swapping its value type calls neither
non-trivial special member function nor ADL-found `swap`.

### Possible implementation

```cpp
template<class ForwardIt1, class ForwardIt2>
constexpr ForwardIt2 swap_ranges(ForwardIt1 first1, ForwardIt1 last1, ForwardIt2 first2)
{
    for (; first1 != last1; ++first1, ++first2)
        std::iter_swap(first1, first2);

    return first2;
}
```

### Example

Demonstrates swapping of subranges from different containers.

```cpp
#include <algorithm>
#include <iostream>
#include <list>
#include <vector>

auto print = [](auto comment, auto const& seq)
{
    std::cout << comment;
    for (const auto& e : seq)
        std::cout << e << ' ';
    std::cout << '\n';
};

int main()
{
    std::vector<char> v{'a', 'b', 'c', 'd', 'e'};
    std::list<char> l{'1', '2', '3', '4', '5'};

    print("Before swap_ranges:\n" "v: ", v);
    print("l: ", l);

    std::swap_ranges(v.begin(), v.begin() + 3, l.begin());

    print("After swap_ranges:\n" "v: ", v);
    print("l: ", l);
}
```

Output:

```text
Before swap_ranges:
v: a b c d e
l: 1 2 3 4 5
After swap_ranges:
v: 1 2 3 d e
l: a b c 4 5
```

### See also

- **iter_swap** — swaps the elements pointed to by two iterators (function
  template)
- **swap** — swaps the values of two objects (function template)
- **ranges::swap_ranges (C++20)** — swaps two ranges of elements (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/swap_ranges*
