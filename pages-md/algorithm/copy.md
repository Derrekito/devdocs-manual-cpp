# std::copy, std::copy_if

```cpp
template< class InputIt, class OutputIt >
OutputIt copy( InputIt first, InputIt last, OutputIt d_first );  // (1) (constexpr since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2 >
ForwardIt2 copy( ExecutionPolicy&& policy,
                 ForwardIt1 first, ForwardIt1 last, ForwardIt2 d_first );  // (2) (since C++17)
template< class InputIt, class OutputIt, class UnaryPredicate >
OutputIt copy_if( InputIt first, InputIt last, OutputIt d_first,
                  UnaryPredicate pred );  // (3) (since C++11) (constexpr since C++20)
template< class ExecutionPolicy,
          class ForwardIt1, class ForwardIt2, class UnaryPredicate >
ForwardIt2 copy_if( ExecutionPolicy&& policy,
                    ForwardIt1 first, ForwardIt1 last, ForwardIt2 d_first,
                    UnaryPredicate pred );  // (4) (since C++17)
```

Copies the elements in the range, defined by `[``first``,``last``)`, to another
range beginning at `d_first` (copy destination range).

1) Copies all elements in the range `[``first``,``last``)` starting from `first`
   and proceeding to `last`.

If `d_first` is in `[``first``,``last``)`, the behavior is undefined. In this
   case, `std::copy_backward` may be used instead.

2) Copies the elements, but executed according to `policy`. This overload does
   not participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

If `[``first``,``last``)` and the copy destination range overlaps, the behavior
   is undefined.

3) Only copies the elements for which the predicate `pred` returns `true`. This
   copy algorithm is stable: the relative order of the elements that are copied
   is preserved.

If `[``first``,``last``)` and the copy destination range overlaps, the behavior
   is undefined.

4) Same as (3), but executed according to `policy`. This overload does not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — the range of elements to copy
- **d_first** — the beginning of the destination range
- **policy** — the execution policy to use. See execution policy for details.
- **pred** — unary predicate which returns ​`true` for the required elements.
  The expression `pred(v)` must be convertible to `bool` for every argument `v`
  of type (possibly const) `VT`, where `VT` is the value type of `InputIt`,
  regardless of value category, and must not modify `v`. Thus, a parameter type
  of `VT&`is not allowed, nor is `VT` unless for `VT` a move is equivalent to a
  copy(since C++11). ​

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-`OutputIt` must meet the requirements of LegacyOutputIterator.**

**-`ForwardIt1, ForwardIt2` must meet the requirements of LegacyForwardIterator.**

**-`UnaryPredicate` must meet the requirements of Predicate.**

### Return value

Output iterator to the element in the destination range, one past the last
element copied.

### Complexity

Given \(\scriptsize N\)N as `std::distance(first, last)`:

1,2) Exactly \(\scriptsize N\)N assignments.

3,4) Exactly \(\scriptsize N\)N applications of the predicate `pred`, and at
   most NN assignments.

For the overloads with an `ExecutionPolicy`, there may be a performance cost if
`ForwardIt1`'s value type is not MoveConstructible.

### Exceptions

The overloads with a template parameter named `ExecutionPolicy` report errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

In practice, implementations of `std::copy` avoid multiple assignments and use
bulk copy functions such as `std::memmove` if the value type is
TriviallyCopyable and the iterator types satisfy LegacyContiguousIterator.

When copying overlapping ranges, `std::copy` is appropriate when copying to the
left (beginning of the destination range is outside the source range) while
`std::copy_backward` is appropriate when copying to the right (end of the
destination range is outside the source range).

### Possible implementation

```cpp
template<class InputIt, class OutputIt>
OutputIt copy(InputIt first, InputIt last,
              OutputIt d_first)
{
    for (; first != last; (void)++first, (void)++d_first)
        *d_first = *first;

    return d_first;
}
```

```cpp
template<class InputIt, class OutputIt, class UnaryPredicate>
OutputIt copy_if(InputIt first, InputIt last,
                 OutputIt d_first, UnaryPredicate pred)
{
    for (; first != last; ++first)
        if (pred(*first))
        {
            *d_first = *first;
            ++d_first;
        }

    return d_first;
}
```

### Example

The following code uses `std::copy` to both copy the contents of one
`std::vector` to another and to display the resulting `std::vector`.

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <numeric>
#include <vector>

int main()
{
    std::vector<int> from_vector(10);
    std::iota(from_vector.begin(), from_vector.end(), 0);

    std::vector<int> to_vector;
    std::copy(from_vector.begin(), from_vector.end(),
              std::back_inserter(to_vector));
// or, alternatively,
//  std::vector<int> to_vector(from_vector.size());
//  std::copy(from_vector.begin(), from_vector.end(), to_vector.begin());
// either way is equivalent to
//  std::vector<int> to_vector = from_vector;

    std::cout << "to_vector contains: ";

    std::copy(to_vector.begin(), to_vector.end(),
              std::ostream_iterator<int>(std::cout, " "));
    std::cout << '\n';

    std::cout << "odd numbers in to_vector are: ";

    std::copy_if(to_vector.begin(), to_vector.end(),
                 std::ostream_iterator<int>(std::cout, " "),
                 [](int x) { return x % 2 != 0; });
    std::cout << '\n';

    std::cout << "to_vector contains these multiples of 3: ";

    to_vector.clear();
    std::copy_if(from_vector.begin(), from_vector.end(),
                 std::back_inserter(to_vector),
                 [](int x) { return x % 3 == 0; });

    for (const int x : to_vector)
        std::cout << x << ' ';
    std::cout << '\n';
}
```

Possible output:

```text
to_vector contains: 0 1 2 3 4 5 6 7 8 9
odd numbers in to_vector are: 1 3 5 7 9
to_vector contains these multiples of 3: 0 3 6 9
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2039 | C++11 | the return value of `std::copy_if` was not specified |
      specified
  LWG 2044 | C++11 | the stability of `std::copy_if` was not defined | defined

### See also

- **copy_backward** — copies a range of elements in backwards order (function
  template)
- **reverse_copy** — creates a copy of a range that is reversed (function
  template)
- **copy_n (C++11)** — copies a number of elements to a new location (function
  template)
- **fill** — copy-assigns the given value to every element in a range (function
  template)
- **remove_copyremove_copy_if** — copies a range of elements omitting those that
  satisfy specific criteria (function template)
- **ranges::copyranges::copy_if (C++20)(C++20)** — copies a range of elements to
  a new location (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/copy*
