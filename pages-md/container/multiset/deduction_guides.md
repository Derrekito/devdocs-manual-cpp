# deduction guides for `std::multiset`

```cpp
template<
    class InputIt,
    class Comp = std::less<typename std::iterator_traits<InputIt>::value_type>,
    class Alloc = std::allocator<typename std::iterator_traits<InputIt>::value_type>>
multiset( InputIt, InputIt, Comp = Comp(), Alloc = Alloc() )
    -> multiset<typename std::iterator_traits<InputIt>::value_type, Comp, Alloc>;  // (1) (since C++17)
template<
    class Key, class Comp = std::less<Key>,
    class Alloc = std::allocator<Key> >
multiset( std::initializer_list<Key>, Comp = Comp(), Alloc = Alloc() )
    -> multiset<Key, Comp, Alloc>;  // (2) (since C++17)
template< class InputIt, class Alloc >
multiset( InputIt, InputIt, Alloc )
    -> multiset<typename std::iterator_traits<InputIt>::value_type,
                std::less<typename std::iterator_traits<InputIt>::value_type>, Alloc>;  // (3) (since C++17)
template< class Key, class Alloc >
multiset( std::initializer_list<Key>, Alloc )
    -> multiset<Key, std::less<Key>, Alloc>;  // (4) (since C++17)
template< ranges::input_range R, class Compare = less<ranges::range_value_t<R>>,
          class Alloc = std::allocator<ranges::range_value_t<R>> >
multiset( std::from_range_t, R&&, Compare = Compare(), Alloc = Alloc() )
    -> multiset<ranges::range_value_t<R>, Compare, Alloc>;  // (5) (since C++23)
template< ranges::input_range R, class Alloc >
multiset( std::from_range_t, R&&, Alloc )
    -> multiset<ranges::range_value_t<R>, std::less<ranges::range_value_t<R>>, Alloc>;  // (6) (since C++23)
```

1-4) These deduction guides are provided for `multiset` to allow deduction from
   an iterator range (overloads (1,3)) and `std::initializer_list` (overloads
   (2,4)).

5,6) These deduction guides are provided for `multiset` to allow deduction from
   a `std::from_range_t` tag and an `input_range`.

These overloads participate in overload resolution only if `InputIt` satisfies
LegacyInputIterator, `Alloc` satisfies Allocator, and `Comp` does not satisfy
Allocator.

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
      and insertion; overload (5,6)

### Example

```cpp
#include <set>

int main()
{
    // guide #2 deduces std::multiset<int>
    std::multiset s = {1, 2, 3, 4};

    // guide #1 deduces std::multiset<int>
    std::multiset s2(s.begin(), s.end());
}
```

---
*Source: https://en.cppreference.com/w/cpp/container/multiset/deduction_guides*
