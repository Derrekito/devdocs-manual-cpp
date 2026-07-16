# std::next

```cpp
template< class InputIt >
InputIt next( InputIt it, typename std::iterator_traits<InputIt>::difference_type n = 1 );  // (since C++11) (until C++17)
template< class InputIt >
constexpr
InputIt next( InputIt it, typename std::iterator_traits<InputIt>::difference_type n = 1 );  // (since C++17)
```

Return the `n`th successor (or `-n`th predecessor if `n` is negative) of
iterator `it`.

### Parameters

- **it** — an iterator
- **n** — number of elements to advance

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

### Return value

An iterator of type `InputIt` that holds the `n`th successor (or `-n`th
predecessor if `n` is negative) of iterator `it`.

### Complexity

Linear.

However, if `InputIt` additionally meets the requirements of
LegacyRandomAccessIterator, complexity is constant.

### Possible implementation

```cpp
template<class InputIt>
constexpr // since C++17
InputIt next(InputIt it, typename std::iterator_traits<InputIt>::difference_type n = 1)
{
    std::advance(it, n);
    return it;
}
```

### Notes

Although the expression `++c.begin()` often compiles, it is not guaranteed to do
so: `c.begin()` is an rvalue expression, and there is no LegacyInputIterator
requirement that specifies that increment of an rvalue is guaranteed to work. In
particular, when iterators are implemented as pointers or its `operator++` is
lvalue-ref-qualified, `++c.begin()` does not compile, while
`std::next(c.begin())` does.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v{4, 5, 6};

    auto it = v.begin();
    auto nx = std::next(it, 2);
    std::cout << *it << ' ' << *nx << '\n';

    it = v.end();
    nx = std::next(it, -2);
    std::cout << ' ' << *nx << '\n';
}
```

Output:

```text
4 6
 5
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2353 | C++11 | `next` required LegacyForwardIterator | LegacyInputIterator
      allowed

### See also

- **prev (C++11)** — decrement an iterator (function template)
- **advance** — advances an iterator by given distance (function template)
- **distance** — returns the distance between two iterators (function template)
- **ranges::next (C++20)** — increment an iterator by a given distance or to a
  bound (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/iterator/next*
