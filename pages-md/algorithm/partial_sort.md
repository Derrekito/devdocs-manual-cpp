# std::partial_sort

```cpp
template< class RandomIt >
void partial_sort( RandomIt first, RandomIt middle, RandomIt last );  // (until C++20)
template< class RandomIt >
constexpr void partial_sort( RandomIt first, RandomIt middle, RandomIt last );  // (since C++20)
template< class ExecutionPolicy, class RandomIt >
void partial_sort( ExecutionPolicy&& policy,
                   RandomIt first, RandomIt middle, RandomIt last );  // (2) (since C++17)
template< class RandomIt, class Compare >
void partial_sort( RandomIt first, RandomIt middle, RandomIt last,
                   Compare comp );  // (until C++20)
template< class RandomIt, class Compare >
constexpr void partial_sort( RandomIt first, RandomIt middle, RandomIt last,
                             Compare comp );  // (since C++20)
template< class ExecutionPolicy, class RandomIt, class Compare >
void partial_sort( ExecutionPolicy&& policy,
                   RandomIt first, RandomIt middle, RandomIt last,
                   Compare comp );  // (4) (since C++17)
```

Rearranges elements such that the range `[``first``,``middle``)` contains the
sorted `middle − first` smallest elements in the range `[``first``,``last``)`.

The order of equal elements is not guaranteed to be preserved. The order of the
remaining elements in the range `[``middle``,``last``)` is unspecified.

1) Elements are compared using `operator<`.

3) Elements are compared using the given binary comparison function `comp`.

2,4) Same as (1,3), but executed according to `policy`. These overloads do not
   participate in overload resolution unless
   `std::is_execution_policy_v<std::decay_t<ExecutionPolicy>>` is `true`. (until
   C++20) `std::is_execution_policy_v<std::remove_cvref_t<ExecutionPolicy>>` is
   `true`. (since C++20)

### Parameters

- **first, last** — random access iterators defining the range
- **middle** — random access iterator defining the one-past-the-end iterator of
  the range to be sorted
- **policy** — the execution policy to use. See execution policy for details.
- **comp** — comparison function object (i.e. an object that satisfies the
  requirements of Compare) which returns ​`true` if the first argument is *less*
  than (i.e. is ordered *before*) the second. The signature of the comparison
  function should be equivalent to the following: `bool cmp(const Type1& a,
  const Type2& b);` While the signature does not need to have const&, the
  function must not modify the objects passed to it and must be able to accept
  all values of type (possibly const) `Type1` and `Type2` regardless of value
  category (thus, `Type1&` is not allowed, nor is `Type1` unless for `Type1` a
  move is equivalent to a copy(since C++11)). The types Type1 and Type2 must be
  such that an object of type RandomIt can be dereferenced and then implicitly
  converted to both of them. ​

**Type requirements**

**-`RandomIt` must meet the requirements of ValueSwappable and LegacyRandomAccessIterator.**

**-The type of dereferenced `RandomIt` must meet the requirements of MoveAssignable and MoveConstructible.**

### Return value

(none)

### Complexity

Approximately (last-first)log(middle-first) applications of `cmp`.

### Exceptions

The overloads with a template parameter named `ExecutionPolicy` report errors as
follows:

- If execution of a function invoked as part of the algorithm throws an
  exception and `ExecutionPolicy` is one of the standard policies,
  `std::terminate` is called. For any other `ExecutionPolicy`, the behavior is
  implementation-defined.
- If the algorithm fails to allocate memory, `std::bad_alloc` is thrown.

### Notes

#### Algorithm

The algorithm used is typically *heap select* to select the smallest elements,
and *heap sort* to sort the selected elements in the heap in ascending order.

To select elements, a heap is used (see heap). For example, for `operator<` as
comparison function, *max-heap* is used to select `middle − first` smallest
elements.

Heap sort is used after selection to sort `[``first``,``middle``)` selected
elements (see `std::sort_heap`).

#### Intended use

`std::partial_sort` algorithms are intended to be used for *small constant
numbers* of `[``first``,``middle``)` selected elements.

### Possible implementation

See also the implementations in libstdc++ and libc++.

```cpp
template<typename RandomIt>
// constexpr since C++20
void partial_sort(RandomIt first, RandomIt middle, RandomIt last)
{
    typedef typename std::iterator_traits<RandomIt>::value_type VT;
    std::partial_sort(first, middle, last, std::less<VT>());
}
```

```cpp
namespace impl
{
    template<typename RandomIt, typename Compare>
    // constexpr
    void sift_down(RandomIt first, RandomIt last, const Compare& comp)
    {
        // sift down element at 'first'
        const auto length = static_cast<size_t>(last - first);
        std::size_t current = 0;
        std::size_t next = 2;
        while (next < length)
        {
            if (comp(*(first + next), *(first + (next - 1))))
                --next;
            if (!comp(*(first + current), *(first + next)))
                return;
            std::iter_swap(first + current, first + next);
            current = next;
            next = 2 * current + 2;
        }
        --next;
        if (next < length && comp(*(first + current), *(first + next)))
            std::iter_swap(first + current, first + next);
    }

    template<typename RandomIt, typename Compare>
    // constexpr
    void heap_select(RandomIt first, RandomIt middle, RandomIt last, const Compare& comp)
    {
        std::make_heap(first, middle, comp);
        for (auto i = middle; i != last; ++i)
        {
            if (comp(*i, *first))
            {
                std::iter_swap(first, i);
                sift_down(first, middle, comp);
            }
        }
    }
} // namespace impl

template<typename RandomIt, typename Compare>
// constexpr since C++20
void partial_sort(RandomIt first, RandomIt middle, RandomIt last, Compare comp)
{
    impl::heap_select(first, middle, last, comp);
    std::sort_heap(first, middle, comp);
}
```

### Example

```cpp
#include <algorithm>
#include <array>
#include <functional>
#include <iostream>

void print(auto const& s, int middle)
{
    for (int a : s)
        std::cout << a << ' ';
    std::cout << '\n';
    if (middle > 0)
    {
        while (middle-- > 0)
            std::cout << "--";
        std::cout << '^';
    }
    else if (middle < 0)
    {
        for (auto i = s.size() + middle; --i; std::cout << "  ")
        {}

        for (std::cout << '^'; middle++ < 0; std::cout << "--")
        {}
    }
    std::cout << '\n';
};

int main()
{
    std::array<int, 10> s{5, 7, 4, 2, 8, 6, 1, 9, 0, 3};
    print(s, 0);
    std::partial_sort(s.begin(), s.begin() + 3, s.end());
    print(s, 3);
    std::partial_sort(s.rbegin(), s.rbegin() + 4, s.rend());
    print(s, -4);
    std::partial_sort(s.rbegin(), s.rbegin() + 5, s.rend(), std::greater{});
    print(s, -5);
}
```

Possible output:

```text
5 7 4 2 8 6 1 9 0 3

0 1 2 7 8 6 5 9 4 3
------^
4 5 6 7 8 9 3 2 1 0
          ^--------
4 3 2 1 0 5 6 7 8 9
        ^----------
```

### See also

- **nth_element** — partially sorts the given range making sure that it is
  partitioned by the given element (function template)
- **partial_sort_copy** — copies and partially sorts a range of elements
  (function template)
- **stable_sort** — sorts a range of elements while preserving order between
  equal elements (function template)
- **sort** — sorts a range into ascending order (function template)
- **ranges::partial_sort (C++20)** — sorts the first N elements of a range
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/partial_sort*
