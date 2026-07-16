# std::iterator_traits

```cpp
template< class Iter >
struct iterator_traits;
template< class T >
struct iterator_traits<T*>;
template< class T >
struct iterator_traits<const T*>;  // (removed in C++20)
```

`std::iterator_traits` is the type trait class that provides uniform interface
to the properties of LegacyIterator types. This makes it possible to implement
algorithms only in terms of iterators.

The template can be specialized for user-defined iterators so that the
information about the iterator can be retrieved even if the type does not
provide the usual typedefs.

User specializations may define the member type `iterator_concept` to one of
iterator category tags, to indicate conformance to the iterator concepts.
*(since C++20)*

### Template parameters

- **Iter** — the iterator type to retrieve properties for

### Member types

- **`difference_type`** — `Iter::difference_type`
- **`value_type`** — `Iter::value_type`
- **`pointer`** — `Iter::pointer`
- **`reference`** — `Iter::reference`
- **`iterator_category`** — `Iter::iterator_category`

If `Iter` does not have all five member types `difference_type`, `value_type`,
`pointer`, `reference`, and `iterator_category`, then this template has no
members by any of those names (`std::iterator_traits` is SFINAE-friendly).
*(since C++17)(until C++20)*

If `Iter` does not have `pointer`, but has all four remaining member types, then
the member types are declared as follows:
- **`difference_type`** — `Iter::difference_type`
- **`value_type`** — `Iter::value_type`
- **`pointer`** — `void`
- **`reference`** — `Iter::reference`
- **`iterator_category`** — `Iter::iterator_category`
Otherwise, if `Iter` satisfies the exposition-only concept
`__LegacyInputIterator`, the member types are declared as follows:
- **`difference_type`** — `std::incrementable_traits<Iter>::difference_type`
- **`value_type`** — `std::indirectly_readable_traits<Iter>::value_type`
- **`pointer`** — `Iter::pointer` if valid, otherwise
  `decltype(std::declval<Iter&>().operator->())` if valid, otherwise `void`
- **`reference`** — `Iter::reference` if valid, otherwise
  `std::iter_reference_t<Iter>`
- **`iterator_category`** — `Iter::iterator_category` if valid, otherwise,
  `std::random_access_iterator_tag` if `Iter` satisfies
  `__LegacyRandomAccessIterator`, otherwise, `std::bidirectional_iterator_tag`
  if `Iter` satisfies `__LegacyBidirectionalIterator`, otherwise,
  `std::forward_iterator_tag` if `Iter` satisfies `__LegacyForwardIterator`,
  otherwise, `std::input_iterator_tag`
Otherwise, if `Iter` satisfies the exposition-only concept `__LegacyIterator`,
the member types are declared as follows:
- **`difference_type`** — `std::incrementable_traits<Iter>::difference_type` if
  valid, otherwise `void`
- **`value_type`** — `void`
- **`pointer`** — `void`
- **`reference`** — `void`
- **`iterator_category`** — `std::output_iterator_tag`
Otherwise, this template has no members by any of those names
(`std::iterator_traits` is SFINAE-friendly).
*(since C++20)*

### Specializations

This type trait may be specialized for user-provided types that may be used as
iterators. The standard library provides partial specializations for pointer
types `T*`, which makes it possible to use all iterator-based algorithms with
raw pointers.

The standard library also provides partial specializations for some standard
iterator adaptors.
*(since C++20)*

#### `T*` specialization member types

Only specialized if `std::is_object_v<T>` is true.
*(since C++20)*

- **`difference_type`** — `std::ptrdiff_t`
- **`value_type`** — `T`(until C++20)`std::remove_cv_t<T>`(since C++20)
- **`pointer`** — `T*`
- **`reference`** — `T&`
- **`iterator_category`** — `std::random_access_iterator_tag`
- **`iterator_concept`(C++20)** — `std::contiguous_iterator_tag`

#### `const T*` specialization member types
- **`difference_type`** — `std::ptrdiff_t`
- **`value_type`** — `T`
- **`pointer`** — `const T*`
- **`reference`** — `const T&`
- **`iterator_category`** — `std::random_access_iterator_tag`
*(until C++20)*

#### Specializations for library types

- **std::iterator_traits<std::common_iterator> (C++20)** — provides uniform
  interface to the properties of the `std::common_iterator` type (class template
  specialization)
- **std::iterator_traits<std::counted_iterator> (C++20)** — provides uniform
  interface to the properties of the `std::counted_iterator` type (class
  template specialization)

### Example

Shows a general-purpose `std::reverse()` implementation for bidirectional
iterators.

```cpp
#include <iostream>
#include <iterator>
#include <list>
#include <vector>

template<class BidirIt>
void my_reverse(BidirIt first, BidirIt last)
{
    typename std::iterator_traits<BidirIt>::difference_type n = std::distance(first, last);
    for (--n; n > 0; n -= 2)
    {
        typename std::iterator_traits<BidirIt>::value_type tmp = *first;
        *first++ = *--last;
        *last = tmp;
    }
}

int main()
{
    std::vector<int> v{1, 2, 3, 4, 5};
    my_reverse(v.begin(), v.end());
    for (int n : v)
        std::cout << n << ' ';
    std::cout << '\n';

    std::list<int> l{1, 2, 3, 4, 5};
    my_reverse(l.begin(), l.end());
    for (int n : l)
        std::cout << n << ' ';
    std::cout << '\n';

    int a[]{1, 2, 3, 4, 5};
    my_reverse(a, a + std::size(a));
    for (int n : a)
        std::cout << n << ' ';
    std::cout << '\n';

//  std::istreambuf_iterator<char> i1(std::cin), i2;
//  my_reverse(i1, i2); // compilation error: i1, i2 are input iterators
}
```

Output:

```text
5 4 3 2 1
5 4 3 2 1
5 4 3 2 1
```

### See also

- **iterator (deprecated in C++17)** — base class to ease the definition of
  required types for simple iterators (class template)
-
  **input_iterator_tagoutput_iterator_tagforward_iterator_tagbidirectional_iterator_tagrandom_access_iterator_tagcontiguous_iterator_tag
  (C++20)** — empty class types used to indicate iterator categories (class)
-
  **iter_value_titer_reference_titer_const_reference_titer_difference_titer_rvalue_reference_titer_common_reference_t
  (C++20)(C++20)(C++23)(C++20)(C++20)(C++20)** — computes the associated types
  of an iterator (alias template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/iterator_traits*
