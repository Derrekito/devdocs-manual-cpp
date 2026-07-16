# std::iter_swap

```cpp
template< class ForwardIt1, class ForwardIt2 >
void iter_swap( ForwardIt1 a, ForwardIt2 b );  // (constexpr since C++20)
```

Swaps the values of the elements the given iterators are pointing to.

### Parameters

- **a, b** — iterators to the elements to swap

**Type requirements**

**-`ForwardIt1, ForwardIt2` must meet the requirements of LegacyForwardIterator.**

**-`*a, *b` must meet the requirements of Swappable.**

### Return value

(none)

### Complexity

Constant.

### Notes

This function template models the semantics of the swap operation given by
Swappable. That is, overloads of `swap` found by ADL and the fall back of
`std::swap` are considered.

### Possible implementation

```cpp
template<class ForwardIt1, class ForwardIt2>
constexpr void iter_swap(ForwardIt1 a, ForwardIt2 b) // constexpr since C++20
{
    using std::swap;
    swap(*a, *b);
}
```

### Example

The following is an implementation of selection sort in C++.

```cpp
#include <algorithm>
#include <iostream>
#include <random>
#include <string_view>
#include <vector>

template<class ForwardIt>
void selection_sort(ForwardIt begin, ForwardIt end)
{
    for (ForwardIt it = begin; it != end; ++it)
        std::iter_swap(it, std::min_element(it, end));
}

void println(std::string_view rem, std::vector<int> const& v)
{
    std::cout << rem;
    for (int e : v)
        std::cout << e << ' ';
    std::cout << '\n';
}

template<int min, int max>
int rand_int()
{
    static std::uniform_int_distribution dist(min, max);
    static std::mt19937 gen(std::random_device{}());
    return dist(gen);
}

int main()
{
    std::vector<int> v;
    std::generate_n(std::back_inserter(v), 20, rand_int<-9, +9>);

    std::cout << std::showpos;
    println("Before sort: ", v);
    selection_sort(v.begin(), v.end());
    println("After sort:  ", v);
}
```

Possible output:

```text
Before sort: -9 -3 +2 -8 +0 -1 +8 -4 -5 +1 -4 -5 +4 -9 -8 -6 -6 +8 -4 -6
After sort:  -9 -9 -8 -8 -6 -6 -6 -5 -5 -4 -4 -4 -3 -1 +0 +1 +2 +4 +8 +8
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 187 | C++98 | it was unspecified whether `swap` is used | the effect is
      equivalent to `swap(*a, *b)`

### See also

- **swap** — swaps the values of two objects (function template)
- **swap_ranges** — swaps two ranges of elements (function template)
- **iter_swap (C++20)** — swaps the objects pointed to by two adjusted
  underlying iterators (function template)
- **iter_swap (C++20)** — swaps the objects pointed to by two underlying
  iterators (function template)
- **iter_swap (C++20)** — swaps the values referenced by two dereferenceable
  objects (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/iter_swap*
