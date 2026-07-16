# deduction guides for `std::unordered_multimap`

```cpp
template< class InputIt,
          class Hash = std::hash<iter_key_t<InputIt>>,
          class Pred = std::equal_to<iter_key_t<InputIt>>,
          class Alloc = std::allocator<iter_to_alloc_t<InputIt>> >
unordered_multimap( InputIt, InputIt,
                    typename /*see below*/::size_type = /*see below*/,
                    Hash = Hash(), Pred = Pred(), Alloc = Alloc() )
    -> unordered_multimap<iter_key_t<InputIt>, iter_val_t<InputIt>,
                          Hash, Pred, Alloc>;  // (1) (since C++17)
template< class Key, class T, class Hash = std::hash<Key>,
          class Pred = std::equal_to<Key>,
          class Alloc = std::allocator<std::pair<const Key, T>> >
unordered_multimap( std::initializer_list<std::pair<Key, T>>,
                    typename /*see below*/::size_type = /*see below*/,
                    Hash = Hash(), Pred = Pred(), Alloc = Alloc() )
    -> unordered_multimap<Key, T, Hash, Pred, Alloc>;  // (2) (since C++17)
template< class InputIt, class Alloc >
unordered_multimap( InputIt, InputIt, typename /*see below*/::size_type, Alloc )
    -> unordered_multimap<iter_key_t<InputIt>, iter_val_t<InputIt>,
                          std::hash<iter_key_t<InputIt>>,
                          std::equal_to<iter_key_t<InputIt>>, Alloc>;  // (3) (since C++17)
template< class InputIt, class Alloc >
unordered_multimap( InputIt, InputIt, Alloc )
    -> unordered_multimap<iter_key_t<InputIt>, iter_val_t<InputIt>,
                          std::hash<iter_key_t<InputIt>>,
                          std::equal_to<iter_key_t<InputIt>>, Alloc>;  // (4) (since C++17)
template< class InputIt, class Hash, class Alloc >
unordered_multimap( InputIt, InputIt, typename /*see below*/::size_type, Hash, Alloc )
    -> unordered_multimap<iter_key_t<InputIt>, iter_val_t<InputIt>, Hash,
                          std::equal_to<iter_key_t<InputIt>>, Alloc>;  // (5) (since C++17)
template< class Key, class T, typename Alloc >
unordered_multimap( std::initializer_list<std::pair<Key, T>>,
                    typename /*see below*/::size_type, Alloc )
    -> unordered_multimap<Key, T, std::hash<Key>, std::equal_to<Key>, Alloc>;  // (6) (since C++17)
template< class Key, class T, typename Alloc >
unordered_multimap( std::initializer_list<std::pair<Key, T>>, Alloc )
    -> unordered_multimap<Key, T, std::hash<Key>, std::equal_to<Key>, Alloc>;  // (7) (since C++17)
template< class Key, class T, class Hash, class Alloc >
unordered_multimap( std::initializer_list<std::pair<Key, T>>,
                    typename /*see below*/::size_type, Hash, Alloc )
    -> unordered_multimap<Key, T, Hash, std::equal_to<Key>, Alloc>;  // (8) (since C++17)
template< ranges::input_range R,
          class Hash = std::hash<range_key_t<R>>,
          class Pred = std::equal_to<range_key_t<R>>,
          class Alloc = std::allocator<range_to_alloc_t<R>> >
unordered_multimap( std::from_range_t, R&&,
                    typename /* see description */::size_type = /* see description */,
                    Hash = Hash(), Pred = Pred(), Alloc = Alloc() )
    -> unordered_multimap<range_key_t<R>, range_mapped_t<R>,
                          Hash, Pred, Alloc>;  // (9) (since C++23)
template< ranges::input_range R, class Alloc >
unordered_multimap( std::from_range_t, R&&,
                    typename /* see description */::size_type, Alloc )
    -> unordered_multimap<range_key_t<R>, range_mapped_t<R>,
                          std::hash<range_key_t<R>>,
                          std::equal_to<range_key_t<R>>, Alloc>;  // (10) (since C++23)
template< ranges::input_range R, class Alloc >
unordered_multimap( std::from_range_t, R&&, Alloc )
    -> unordered_multimap<range_key_t<R>, range_mapped_t<R>,
                          std::hash<range_key_t<R>>,
                          std::equal_to<range_key_t<R>>, Alloc>;  // (11) (since C++23)
template< ranges::input_range R, class Hash, class Alloc >
unordered_multimap( std::from_range_t, R&&, typename /* see description */::size_type,
                    Hash, Alloc )
    -> unordered_multimap<range_key_t<R>, range_mapped_t<R>, Hash,
                          std::equal_to<range_key_t<R>>, Alloc>;  // (12) (since C++23)
Exposition-only helper type aliases
template< class InputIter >
using iter_val_t =
    typename std::iterator_traits<InputIter>::value_type;  // (exposition only*)
template< class InputIter >
using iter_key_t =
    std::remove_const_t< std::tuple_element_t<0, iter_val_t<InputIter>>>;  // (exposition only*)
template< class InputIter >
using iter_mapped_t =
    std::tuple_element_t<1, iter_val_t<InputIter>>;  // (exposition only*)
template< class InputIter >
using iter_to_alloc_t =
    std::pair<std::add_const_t<tuple_element_t<0, iter_val_t<InputIter>>>,
              std::tuple_element_t<1, iter_val_t<InputIter>>>;  // (exposition only*)
template< ranges::input_range Range >
using range_key_t =
    std::remove_const_t<typename ranges::range_value_t<Range>::first_type>;  // (since C++23) (exposition only*)
template< ranges::input_range Range >
using range_mapped_t =
    typename ranges::range_value_t<Range>::second_type;  // (since C++23) (exposition only*)
template< ranges::input_range Range >
using range_to_alloc_t =
    std::pair<std::add_const_t<typename ranges::range_value_t<Range>::first_type>,
              typename ranges::range_value_t<Range>::second_type>;  // (since C++23) (exposition only*)
```

1-8) These deduction guide are provided for `unordered_multimap` to allow
   deduction from an iterator range (overloads (1,3-5)) and
   `std::initializer_list` (overloads (2,6-8)).

9-12) These deduction guides are provided for `unordered_multimap` to allow
   deduction from a `std::from_range_t` tag and an `input_range`.

These overloads participate in overload resolution only if `InputIt` satisfies
LegacyInputIterator, `Alloc` satisfies Allocator, neither `Hash` nor `Pred`
satisfy Allocator, and `Hash` is not an integral type.

Note: the extent to which the library determines that a type does not satisfy
LegacyInputIterator is unspecified, except that as a minimum integral types do
not qualify as input iterators. Likewise, the extent to which it determines that
a type does not satisfy Allocator is unspecified, except that as a minimum the
member type `Alloc::value_type` must exist and the expression
`std::declval<Alloc&>().allocate(std::size_t{})` must be well-formed when
treated as an unevaluated operand.

The `size_type` parameter type in these guides in an refers to the `size_type`
member type of the type deduced by the deduction guide.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | Ranges-aware construction
      and insertion; overloads (9-12)

### Example

```cpp
#include <unordered_map>

int main()
{
    // std::unordered_multimap m1 = {{"foo", 1}, {"bar", 2}};
        // Error: braced-init-list has no type cannot
        // deduce pair<Key, T> from {"foo", 1} or {"bar", 2}

    std::unordered_multimap m1 = {std::pair{"foo", 2}, {"bar", 3}}; // guide #2
    std::unordered_multimap m2(m1.begin(), m1.end()); // guide #1
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3025 | C++17 | initializer-list guides take `std::pair<const Key, T>` |
      use `std::pair<Key, T>`

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_multimap/deduction_guides*
