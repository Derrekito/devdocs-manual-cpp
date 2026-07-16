# std::lexicographical_compare

```cpp
template< class InputIt1, class InputIt2 >
bool lexicographical_compare( InputIt1 first1, InputIt1 last1,
                              InputIt2 first2, InputIt2 last2 );  // (until C++20)
template< class InputIt1, class InputIt2 >
constexpr bool lexicographical_compare( InputIt1 first1, InputIt1 last1,
                                        InputIt2 first2, InputIt2 last2 );  // (since C++20)
template< class ExecutionPolicy, class ForwardIt1, class ForwardIt2 >
bool lexicographical_compare( ExecutionPolicy&& policy,
                              ForwardIt1 first1, ForwardIt1 last1,
                              ForwardIt2 first2, ForwardIt2 last2 );  // (2) (since C++17)
template< class InputIt1, class InputIt2, class Compare >
bool lexicographical_compare( InputIt1 first1, InputIt1 last1,
                              InputIt2 first2, InputIt2 last2, Compare comp );  // (until C++20)
template< class InputIt1, class InputIt2, class Compare >
constexpr bool lexicographical_compare( InputIt1 first1, InputIt1 last1,
                                        InputIt2 first2, InputIt2 last2,
                                        Compare comp );  // (since C++20)
template< class ExecutionPolicy,
          class ForwardIt1, class ForwardIt2, class Compare >
bool lexicographical_compare( ExecutionPolicy&& policy,
                              ForwardIt1 first1, ForwardIt1 last1,
                              ForwardIt2 first2, ForwardIt2 last2,
                              Compare comp );  // (4) (since C++17)
```

Checks if the first range `[``first1``,``last1``)` is lexicographically *less*
than the second range `[``first2``,``last2``)`.

1) Elements are compared using `operator<`.

3) Elements are compared using the given binary comparison function `comp`.

2,4) Same as (1,3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

Lexicographical comparison is an operation with the following properties:

- Two ranges are compared element by element.
- The first mismatching element defines which range is lexicographically *less*
  or *greater* than the other.
- If one range is a prefix of another, the shorter range is lexicographically
  *less* than the other.
- If two ranges have equivalent elements and are of the same length, then the
  ranges are lexicographically *equal*.
- An empty range is lexicographically *less* than any non-empty range.
- Two empty ranges are lexicographically *equal*.

### Parameters

- **first1, last1** — the first range of elements to examine
- **first2, last2** — the second range of elements to examine
- **policy** — the execution policy to use. See execution policy for details.
- **comp** — comparison function object (i.e. an object that satisfies the
  requirements of Compare) which returns `true` if the first argument is *less*
  than the second. The signature of the comparison function should be equivalent
  to the following: `bool cmp(const Type1& a, const Type2& b);` While the
  signature does not need to have `const&`, the function must not modify the
  objects passed to it and must be able to accept all values of type (possibly
  const) `Type1` and `Type2` regardless of value category (thus, `Type1&` is not
  allowed, nor is `Type1` unless for `Type1` a move is equivalent to a
  copy(since C++11)). The types Type1 and Type2 must be such that objects of
  types InputIt1 and InputIt2 can be dereferenced and then implicitly converted
  to both Type1 and Type2.

**Type requirements**

**-`InputIt1, InputIt2` must meet the requirements of LegacyInputIterator.**

**-`ForwardIt1, ForwardIt2` must meet the requirements of LegacyForwardIterator.**

**-`Compare` must meet the requirements of Compare.**

### Return value

`true` if the first range is lexicographically *less* than the second, otherwise
`false`.

### Complexity

Given \(\scriptsize N_1\)N1 as `std::distance(first1, last1)` and \(\scriptsize
N_2\)N2 as `std::distance(first2, last2)`:

1,2) At most \(\scriptsize min(N_1,N_2)\)min(N1,N2) comparisons using
   `operator<`.

3,4) At most \(\scriptsize min(N_1,N_2)\)min(N1,N2) applications of the
   comparison function `comp`.

### Exceptions

The overloads with a template parameter named `ExecutionPolicy` report errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Possible implementation

```cpp
template<class InputIt1, class InputIt2>
bool lexicographical_compare(InputIt1 first1, InputIt1 last1,
                             InputIt2 first2, InputIt2 last2)
{
    for (; (first1 != last1) && (first2 != last2); ++first1, (void) ++first2)
    {
        if (*first1 < *first2)
            return true;
        if (*first2 < *first1)
            return false;
    }

    return (first1 == last1) && (first2 != last2);
}
```

```cpp
template<class InputIt1, class InputIt2, class Compare>
bool lexicographical_compare(InputIt1 first1, InputIt1 last1,
                             InputIt2 first2, InputIt2 last2, Compare comp)
{
    for (; (first1 != last1) && (first2 != last2); ++first1, (void) ++first2)
    {
        if (comp(*first1, *first2))
            return true;
        if (comp(*first2, *first1))
            return false;
    }

    return (first1 == last1) && (first2 != last2);
}
```

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <random>
#include <vector>

void print(std::vector<char> const& v, auto suffix)
{
    for (char c : v)
        std::cout << c << ' ';
    std::cout << suffix;
}

int main()
{
    std::vector<char> v1{'a', 'b', 'c', 'd'};
    std::vector<char> v2{'a', 'b', 'c', 'd'};

    for (std::mt19937 g{std::random_device{}()};
         !std::lexicographical_compare(v1.begin(), v1.end(),
                                       v2.begin(), v2.end());)
    {
        print(v1, ">= ");
        print(v2, '\n');

        std::shuffle(v1.begin(), v1.end(), g);
        std::shuffle(v2.begin(), v2.end(), g);
    }

    print(v1, "<  ");
    print(v2, '\n');
}
```

Possible output:

```text
a b c d >= a b c d
d a b c >= c b d a
b d a c >= a d c b
a c d b <  c d a b
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 142 | C++98 | at most \(\scriptsize min(N_1,N_2)\)min(N1,N2) comparisons
      were allowed, but that is not possible (equality is determined by 2
      comparisons) | doubled the limit
  LWG 1205 | C++98 | the results of lexicographical comparisons involving empty
      ranges were unclear | made clear

### See also

- **equal** — determines if two sets of elements are the same (function
  template)
- **lexicographical_compare_three_way (C++20)** — compares two ranges using
  three-way comparison (function template)
- **ranges::lexicographical_compare (C++20)** — returns `true` if one range is
  lexicographically less than another (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/lexicographical_compare*
