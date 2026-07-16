# std::move_iterator

```cpp
template< class Iter >
class move_iterator;  // (since C++11)
```

`std::move_iterator` is an iterator adaptor which behaves exactly like the
underlying iterator (which must be at least a LegacyInputIterator or model
`input_iterator`(since C++20), or stronger iterator concept(since C++23)),
except that dereferencing converts the value returned by the underlying iterator
into an rvalue. If this iterator is used as an input iterator, the effect is
that the values are moved from, rather than copied from.

### Member types

- **`iterator_type`** — `Iter`
- **`iterator_category`** — `std::iterator_traits<Iter>::iterator_category`
  (until C++20) If `std::iterator_traits<Iter>::iterator_category` is valid and
  denotes a type: if `std::iterator_traits<Iter>::iterator_category` models
  `std::derived_from<std::random_access_iterator_tag>`, member
  `iterator_category` is `std::random_access_iterator_tag`. Otherwise, member
  `iterator_category` is the same type as
  `std::iterator_traits<Iter>::iterator_category`. Otherwise, there is no member
  `iterator_category`. (since C++20)
- **`std::iterator_traits<Iter>::iterator_category`** — (until C++20)
- **If `std::iterator_traits<Iter>::iterator_category` is valid and denotes a
  type: if `std::iterator_traits<Iter>::iterator_category` models
  `std::derived_from<std::random_access_iterator_tag>`, member
  `iterator_category` is `std::random_access_iterator_tag`. Otherwise, member
  `iterator_category` is the same type as
  `std::iterator_traits<Iter>::iterator_category`. Otherwise, there is no member
  `iterator_category`.** — (since C++20)
- **`iterator_concept`** — `std::input_iterator_tag` (since C++20) (until C++23)
  `std::random_access_iterator_tag`, if `Iter` models
  `std::random_access_iterator`. Otherwise, `std::bidirectional_iterator_tag`,
  if `Iter` models `std::bidirectional_iterator`. Otherwise,
  `std::forward_iterator_tag`, if `Iter` models `std::forward_iterator`.
  Otherwise, `std::input_iterator_tag`. (since C++23)
- **`std::input_iterator_tag`** — (since C++20) (until C++23)
- **`std::random_access_iterator_tag`, if `Iter` models
  `std::random_access_iterator`. Otherwise, `std::bidirectional_iterator_tag`,
  if `Iter` models `std::bidirectional_iterator`. Otherwise,
  `std::forward_iterator_tag`, if `Iter` models `std::forward_iterator`.
  Otherwise, `std::input_iterator_tag`.** — (since C++23)
- **`value_type`** — `std::iterator_traits<Iter>::value_type` (until C++20)
  `std::iter_value_t<Iter>` (since C++20)
- **`std::iterator_traits<Iter>::value_type`** — (until C++20)
- **`std::iter_value_t<Iter>`** — (since C++20)
- **`difference_type`** — `std::iterator_traits<Iter>::difference_type` (until
  C++20) `std::iter_difference_t<Iter>` (since C++20)
- **`std::iterator_traits<Iter>::difference_type`** — (until C++20)
- **`std::iter_difference_t<Iter>`** — (since C++20)
- **`pointer`** — `Iter`
- **`reference`** — If `std::iterator_traits<Iter>::reference` is a reference,
  this is the rvalue reference version of the same type. Otherwise (such as if
  the wrapped iterator returns by value), this is
  `std::iterator_traits<Iter>::reference` unchanged. (until C++20)
  `std::iter_rvalue_reference_t<Iter>` (since C++20)
- **If `std::iterator_traits<Iter>::reference` is a reference, this is the
  rvalue reference version of the same type. Otherwise (such as if the wrapped
  iterator returns by value), this is `std::iterator_traits<Iter>::reference`
  unchanged.** — (until C++20)
- **`std::iter_rvalue_reference_t<Iter>`** — (since C++20)

### Member objects

- **`current` (private)** — the underlying iterator from which `base()` copies
  or moves(since C++20) (exposition-only member object*)

### Member functions

- **(constructor) (C++11)** — constructs a new iterator adaptor (public member
  function)
- **operator= (C++11)** — assigns another iterator adaptor (public member
  function)
- **base (C++11)** — accesses the underlying iterator (public member function)
- **operator*operator-> (C++11)(C++11)(deprecated in C++20)** — accesses the
  pointed-to element (public member function)
- **operator[] (C++11)** — accesses an element by index (public member function)
-
  **operator++operator++(int)operator+=operator+operator--operator--(int)operator-=operator-
  (C++11)** — advances or decrements the iterator (public member function)

### Non-member functions

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++11)(C++11)(removed in C++20)(C++11)(C++11)(C++11)(C++11)(C++20)** —
  compares the underlying iterators (function template)
- **operator==(std::move_sentinel) (C++20)** — compares the underlying iterator
  and the underlying sentinel (function template)
- **operator+ (C++11)** — advances the iterator (function template)
- **operator- (C++11)** — computes the distance between two iterator adaptors
  (function template)
- **operator-(std::move_sentinel) (C++20)** — computes the distance between the
  underlying iterator and the underlying sentinel (function template)
- **iter_move (C++20)** — casts the result of dereferencing the underlying
  iterator to its associated rvalue reference type (function)
- **iter_swap (C++20)** — swaps the objects pointed to by two underlying
  iterators (function template)
- **make_move_iterator (C++11)** — creates a `std::move_iterator` of type
  inferred from the argument (function template)

### Helper templates

```cpp
template< class Iterator1, class Iterator2 >
    requires (!std::sized_sentinel_for<Iterator1, Iterator2>)
constexpr bool disable_sized_sentinel_for<
    std::move_iterator<Iterator1>,
    std::move_iterator<Iterator2>> = true;  // (since C++20)
```

This partial specialization of `std::disable_sized_sentinel_for` prevents
specializations of `move_iterator` from satisfying `sized_sentinel_for` if their
underlying iterators do not satisfy the concept.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_move_iterator_concept` | 202207L | (C++23) | Make
      `std::move_iterator<T*>` a random access iterator

### Example

```cpp
#include <algorithm>
#include <iomanip>
#include <iostream>
#include <iterator>
#include <numeric>
#include <string>
#include <vector>

int main()
{
    std::vector<std::string> v{"this", "_", "is", "_", "an", "_", "example"};

    auto print_v = [&](auto const rem)
    {
        std::cout << rem;
        for (const auto& s : v)
            std::cout << std::quoted(s) << ' ';
        std::cout << '\n';
    };

    print_v("Old contents of the vector: ");

    std::string concat = std::accumulate(std::make_move_iterator(v.begin()),
                                         std::make_move_iterator(v.end()),
                                         std::string());

    // An alternative that uses std::move_iterator directly could be:
    // using moviter_t = std::move_iterator<std::vector<std::string>::iterator>;
    // std::string concat = std::accumulate(moviter_t(v.begin()),
    //                                      moviter_t(v.end()),
    //                                      std::string());

    // Starting from C++17, which introduced class template argument deduction,
    // the constructor of std::move_iterator can be used directly without
    // template parameters in most cases:
    // std::string concat = std::accumulate(std::move_iterator(v.begin()),
    //                                      std::move_iterator(v.end()),
    //                                      std::string());

    print_v("New contents of the vector: ");

    std::cout << "Concatenated as string: " << std::quoted(concat) << '\n';
}
```

Possible output:

```text
Old contents of the vector: "this" "_" "is" "_" "an" "_" "example"
New contents of the vector: "" "" "" "" "" "" ""
Concatenated as string: "this_is_an_example"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2106 | C++11 | dereferencing a `move_iterator` could return a dangling
      reference if the dereferencing the underlying iterator returns a prvalue |
      returns the object instead
  LWG 3736 | C++20 | `move_iterator` is missing `disable_sized_sentinel_for`
      specialization | added
  P2259R1 | C++20 | member `iterator_category` was always defined | defined only
      if `std::iterator_traits<Iter>::iterator_category` exists

### See also

- **make_move_iterator (C++11)** — creates a `std::move_iterator` of type
  inferred from the argument (function template)
- **move_sentinel (C++20)** — sentinel adaptor for use with `std::move_iterator`
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/move_iterator*
