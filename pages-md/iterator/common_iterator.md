# std::common_iterator

```cpp
template< std::input_or_output_iterator I, std::sentinel_for<I> S >
    requires ( !std::same_as<I, S> && std::copyable<I> )
        class common_iterator;  // (since C++20)
```

`std::common_iterator` is an iterator `I` / sentinel `S` adaptor that may
represent a non-common range (where the types of `I` and `S` differ) as a
`common_range`, by containing either an iterator or a sentinel, and defining the
appropriate equality comparison operators `operator==`.
`std::common_iterator` can be used as a "bridge" between sequences represented
by iterator/sentinel pair and legacy functions that expect `common_range`-like
sequences.

### Member functions

- **(constructor) (C++20)** — constructs a new iterator adaptor (public member
  function)
- **operator= (C++20)** — assigns another iterator adaptor (public member
  function)
- **operator*operator-> (C++20)** — accesses the pointed-to element (public
  member function)
- **operator++operator++(int) (C++20)** — advances the iterator adaptor (public
  member function)

### Member objects

- **`var` (private)** — an object of type `std::variant<I, S>`, the name is for
  exposition only

### Non-member functions

- **operator== (C++20)** — compares the underlying iterators or sentinels
  (function template)
- **operator- (C++20)** — computes the distance between two iterator adaptors
  (function template)
- **iter_move (C++20)** — casts the result of dereferencing the underlying
  iterator to its associated rvalue reference type (function)
- **iter_swap (C++20)** — swaps the objects pointed to by two underlying
  iterators (function template)

### Helper classes

- **std::incrementable_traits<std::common_iterator> (C++20)** — computes the
  associated difference type of the `std::common_iterator` type (class template
  specialization)
- **std::iterator_traits<std::common_iterator> (C++20)** — provides uniform
  interface to the properties of the `std::common_iterator` type (class template
  specialization)

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <list>
#include <string>

template<class ForwardIter>
void fire(ForwardIter first, ForwardIter last)
{
    std::copy(first, last, std::ostream_iterator<std::string>{std::cout, " "});
}

int main()
{
    std::list<std::string> stars{"Pollux", "Arcturus", "Mira", "Aldebaran", "Sun"};
    using IT = std::common_iterator<
                   std::counted_iterator<std::list<std::string>::iterator>,
                   std::default_sentinel_t>;
    fire(IT(std::counted_iterator(stars.begin(), stars.size()-1)),
         IT(std::default_sentinel));
}
```

Output:

```text
Pollux Arcturus Mira Aldebaran
```

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):

### See also

- **ranges::common_range (C++20)** — specifies that a range has identical
  iterator and sentinel types (concept)
- **ranges::common_viewviews::common (C++20)** — converts a `view` into a
  `common_range` (class template) (range adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/iterator/common_iterator*
