# std::reverse_iterator

```cpp
template< class Iter >
class reverse_iterator;
```

`std::reverse_iterator` is an iterator adaptor that reverses the direction of a
given iterator, which must be at least a LegacyBidirectionalIterator or model
`bidirectional_iterator`(since C++20). In other words, when provided with a
bidirectional iterator, `std::reverse_iterator` produces a new iterator that
moves from the end to the beginning of the sequence defined by the underlying
bidirectional iterator.

For a reverse iterator `r` constructed from an iterator `i`, the relationship
`&*r == &*(i - 1)` is always `true` (as long as `r` is dereferenceable); thus a
reverse iterator constructed from a one-past-the-end iterator dereferences to
the last element in a sequence.

This is the iterator returned by member functions `rbegin()` and `rend()` of the
standard library containers.

### Member types

- **`iterator_type`** — `Iter`
- **`iterator_category`** — std::iterator_traits<Iter>::iterator_category
- **`value_type`** — std::iterator_traits<Iter>::value_type
- **`difference_type`** — std::iterator_traits<Iter>::difference_type
- **`pointer`** — std::iterator_traits<Iter>::pointer
- **`reference`** — std::iterator_traits<Iter>::reference
*(until C++20)*

- **`iterator_type`** — `Iter`
- **`iterator_concept`** — If `Iter` models `std::random_access_iterator`, this
  is `std::random_access_iterator_tag`. Otherwise, this is
  `std::bidirectional_iterator_tag`
- **`iterator_category`** — If std::iterator_traits<Iter>::iterator_category
  models std::derived_from<std::random_access_iterator_tag>, this is
  `std::random_access_iterator_tag`. Otherwise, this is
  std::iterator_traits<Iter>::iterator_category
- **`value_type`** — std::iter_value_t<Iter>
- **`difference_type`** — std::iter_difference_t<Iter>
- **`pointer`** — std::iterator_traits<Iter>::pointer
- **`reference`** — std::iter_reference_t<Iter>
*(since C++20)*

Member types `iterator_category`, `value_type`, `difference_type`, `pointer` and
`reference` are required to be obtained by inheriting from std::iterator<
std::iterator_traits<Iter>::iterator_category,
std::iterator_traits<Iter>::value_type,
std::iterator_traits<Iter>::difference_type,
std::iterator_traits<Iter>::pointer,
std::iterator_traits<Iter>::reference
>.
*(until C++17)*

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

### Member objects

- **`current` (protected)** — the underlying iterator of which `base()` returns
  a copy

### Non-member functions

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++20)** — compares the underlying iterators (function template)
- **operator+** — advances the iterator (function template)
- **operator-** — computes the distance between two iterator adaptors (function
  template)
- **iter_move (C++20)** — casts the result of dereferencing the adjusted
  underlying iterator to its associated rvalue reference type (function)
- **iter_swap (C++20)** — swaps the objects pointed to by two adjusted
  underlying iterators (function template)
- **make_reverse_iterator (C++14)** — creates a `std::reverse_iterator` of type
  inferred from the argument (function template)

### Helper templates

```cpp
template< class Iterator1, class Iterator2 >
    requires (!std::sized_sentinel_for<Iterator1, Iterator2>)
inline constexpr bool disable_sized_sentinel_for<
    std::reverse_iterator<Iterator1>,
    std::reverse_iterator<Iterator2>> = true;  // (since C++20)
```

This partial specialization of `std::disable_sized_sentinel_for` prevents
specializations of `reverse_iterator` from satisfying `sized_sentinel_for` if
their underlying iterators do not satisfy the concept.

### Possible implementation

Below is a partial implementation focusing on the way the inner iterator is
stored, calling `std::prev` only when the content is fetched via operator*.

```cpp
template<class It>
class reverse_iterator
{
protected:
    It current = It();
public:
    reverse_iterator() = default;
    constexpr explicit reverse_iterator(It itr) : current(itr) {}
    template<class U>
        requires (!std::is_same_v<U, It> && std::convertible_to<const U&, It>)
    constexpr explicit reverse_iterator(const U& other) : current(other.base()) {}

    constexpr decltype(auto) operator*() const
    {
        return *std::prev(current); // <== returns the content of prev
    }

    constexpr reverse_iterator& operator++() { --current; return *this; }
    constexpr reverse_iterator operator++(int) { auto tmp = *this; ++(*this); return tmp; }

    constexpr reverse_iterator& operator--() { ++current; return *this; }
    constexpr reverse_iterator operator--(int) { auto tmp = *this; --(*this); return tmp; }

    constexpr It base() const { return current; }

    // Other member functions, friend functions, and member typedefs are not shown here.
};
```

### Notes

`std::reverse_iterator` does not work with iterators whose dereference returns a
reference to a member of `*this` (so-called "stashing iterators"). An example of
a stashing iterator is MSVC STL's `std::filesystem::path::iterator`.

### Example

```cpp
#include <cstddef>
#include <iostream>
#include <iterator>

template<typename T, std::size_t SIZE>
class Stack
{
    T arr[SIZE];
    std::size_t pos = 0;
public:
    T pop()
    {
        return arr[--pos];
    }

    Stack& push(const T& t)
    {
        arr[pos++] = t;
        return *this;
    }

    // we wish that looping on Stack would be in LIFO order
    // thus we use std::reverse_iterator as an adaptor to existing iterators
    // (which are in this case the simple pointers: [arr, arr + pos)
    auto begin() { return std::reverse_iterator(arr + pos); }
    auto end() { return std::reverse_iterator(arr); }
};

int main()
{
    Stack<int, 8> s;
    s.push(5).push(15).push(25).push(35);
    for (int val : s)
        std::cout << val << ' ';
    std::cout << '\n';
}
```

Output:

```text
35 25 15 5
```

### See also

- **make_reverse_iterator (C++14)** — creates a `std::reverse_iterator` of type
  inferred from the argument (function template)
- **iterator (deprecated in C++17)** — base class to ease the definition of
  required types for simple iterators (class template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/reverse_iterator*
