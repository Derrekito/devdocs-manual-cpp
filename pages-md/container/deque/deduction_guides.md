# deduction guides for `std::deque`

```cpp
template< class InputIt,
          class Alloc = std::allocator<
              typename std::iterator_traits<InputIt>::value_type> >
deque( InputIt, InputIt, Alloc = Alloc() )
    -> deque<typename std::iterator_traits<InputIt>::value_type, Alloc>;  // (1) (since C++17)
template< ranges::input_range R,
          class Alloc = std::allocator<ranges::range_value_t<R>> >
deque( std::from_range_t, R&&, Alloc = Alloc() )
    -> deque<ranges::range_value_t<R>, Alloc>;  // (2) (since C++23)
```

1) This deduction guide is provided for deque to allow deduction from an
   iterator range. This overload participates in overload resolution only if
   `InputIt` satisfies LegacyInputIterator and `Alloc` satisfies Allocator.

2) This deduction guide is provided for deque to allow deduction from a
   `std::from_range_t` tag and an `input_range`.

Note: the extent to which the library determines that a type does not satisfy
LegacyInputIterator is unspecified, except that as a minimum integral types do
not qualify as input iterators. Likewise, the extent to which it determines that
a type does not satisfy Allocator is unspecified, except that as a minimum the
member type `Alloc::value_type` must exist and the expression
`std::declval<Alloc&>().allocate(std::size_t{})` must be well-formed when
treated as an unevaluated operand.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | Ranges-aware construction
      and insertion; overload (2)

### Example

```cpp
#include <deque>
#include <vector>

int main()
{
    std::vector<int> v = {1, 2, 3, 4};

    // uses explicit deduction guide to deduce std::deque<int>
    std::deque x(v.begin(), v.end());

    // deduces std::deque<std::vector<int>::iterator>
    // first phase of overload resolution for list-initialization selects the candidate
    // synthesized from the initializer-list constructor; second phase is not performed
    // and deduction guide has no effect
    std::deque y{v.begin(), v.end()};
}
```

---
*Source: https://en.cppreference.com/w/cpp/container/deque/deduction_guides*
