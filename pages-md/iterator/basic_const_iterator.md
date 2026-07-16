# std::basic_const_iterator

```cpp
template< std::input_iterator Iter >
class basic_const_iterator;  // (since C++23)
```

`std::basic_const_iterator` is an iterator adaptor which behaves exactly like
the underlying iterator (which must be at least an LegacyInputIterator or model
`input_iterator`), except that dereferencing converts the value returned by the
underlying iterator as immutable. Specializations of `std::basic_const_iterator`
are constant iterators, that is, the iterator can never be used as an output
iterator because modifying elements is not allowed.

### Member types

- **`iterator_category`** — If `Iter` models `forward_iterator`: member
  `iterator_category` is the same type as
  `std::iterator_traits<Iter>::iterator_category`. Otherwise, there is no member
  `iterator_category`.
- **`iterator_concept`** — `std::contiguous_iterator_tag`, if `Iter` models
  `contiguous_iterator`; `std::random_access_iterator_tag`, if `Iter` models
  `random_access_iterator`; `std::bidirectional_iterator_tag`, if `Iter` models
  `bidirectional_iterator`; `std::forward_iterator_tag`, if `Iter` models
  `forward_iterator`; `std::input_iterator_tag` otherwise.
- **`value_type`** — `std::iter_value_t<Iter>`
- **`difference_type`** — `std::iter_difference_t<Iter>`
- **`reference` (private)** — `std::iter_const_reference_t<Iter>`
  (exposition-only member type*)

### Member objects

- **`current` (private)** — the underlying iterator from which `base()` copies
  or moves (exposition-only member object*)

### Member functions

- **(constructor)** — constructs a new iterator adaptor (public member function)
- **operator=** — assigns another iterator adaptor (public member function)
- **base** — accesses the underlying iterator (public member function)
- **operator*operator->** — accesses the pointed-to element (public member
  function)
- **operator[]** — accesses an element by index (public member function)
-
  **operator++operator++(int)operator+=operator+operator--operator--(int)operator-=operator-**
  — advances or decrements the iterator (public member function)
- **operator *constant-iterator* (C++23)** — converts into any constant iterator
  to which an underlying iterator can be convertible (public member function)

### Non-member functions

- **operator==operator<operator<=operator>operator>=operator<=> (C++23)** —
  compares the underlying iterators (function template)
- **operator+ (C++23)** — advances the iterator (function template)
- **operator- (C++23)** — computes the distance between two iterator adaptors
  (function template)
- **iter_move (C++23)** — casts the result of dereferencing the underlying
  iterator to its associated rvalue reference type (function)

### Helper classes

- **std::common_type<std::basic_const_iterator> (C++23)** — determines the
  common type of an iterator and an adapted `basic_const_iterator` type (class
  template specialization)

### Helper alias templates

```cpp
template< std::input_iterator I >
using const_iterator = /* see description */;  // (since C++23)
```

If `I` models `constant-iterator` (an exposition-only concept), then
`const_iterator<I>` denotes a type `I`. Otherwise, `basic_const_iterator<I>`.

```cpp
template< std::semiregular S >
using const_sentinel = /* see description */;  // (since C++23)
```

If `S` models `input_iterator`, then `const_sentinel<S>` denotes a type
`const_iterator<S>`. Otherwise, `S`.

### Helper function templates

```cpp
template< std::input_iterator T >
constexpr const_iterator<T> make_const_iterator( I it ) { return it; }  // (since C++23)
template< std::semiregular S >
constexpr const_sentinel<S> make_const_sentinel( S s ) { return s; }  // (since C++23)
```

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_as_const` | 202207L | (C++23) | `std::basic_const_iterator`
  202311L | (C++23) (DR) | `std::basic_const_iterator` should follow its
      underlying type's convertibility

### Example

```cpp
#include <cassert>
#include <iterator>
#include <vector>

int main()
{
    std::vector v{1, 2, 3};
    std::vector<int>::iterator i = v.begin();
    *i = 4;   // OK
    i[1] = 4; // OK, the same as *(i + 1) = 4;

    auto ci = std::make_const_iterator(i);
    assert(*ci == 4);   // OK, can read the underlying object
    assert(ci[0] == 4); // OK, ditto
    // *ci = 13;        // Error: location is read-only
    ci.base()[0] = 42;  // OK, underlying iterator is writable
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2836R1 | C++23 | `basic_const_iterator` doesn't follow its underlying type's
      convertibility | conversion operator provided

---
*Source: https://en.cppreference.com/w/cpp/iterator/basic_const_iterator*
