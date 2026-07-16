# std::counted_iterator

```cpp
template< std::input_or_output_iterator I >
class counted_iterator;  // (since C++20)
```

`std::counted_iterator` is an iterator adaptor which behaves exactly like the
underlying iterator, except that it keeps track of the distance to the end of
its range. This iterator is equal to `std::default_sentinel` if and only if its
count reaches zero.

### Member types

- **`iterator_type`** — `I`
- **`value_type`** — `std::iter_value_t<I>` if `I` models `indirectly_readable`;
  otherwise, not defined
- **`difference_type`** — `std::iter_difference_t<I>`
- **`iterator_concept`** — `I::iterator_concept` if present; otherwise, not
  defined
- **`iterator_category`** — `I::iterator_category` if present; otherwise, not
  defined

### Member objects

- **`current` (private)** — the underlying iterator which `base()` accesses
  (exposition-only member object*)
- **`length` (private)** — the distance between the underlying iterator and the
  end of its range (exposition-only member object*)

### Member functions

- **(constructor) (C++20)** — constructs a new iterator adaptor (public member
  function)
- **operator= (C++20)** — assigns another iterator adaptor (public member
  function)
- **base (C++20)** — accesses the underlying iterator (public member function)
- **count (C++20)** — returns the distance to the end (public member function)
- **operator*operator-> (C++20)** — accesses the pointed-to element (public
  member function)
- **operator[] (C++20)** — accesses an element by index (public member function)
-
  **operator++operator++(int)operator+=operator+operator--operator--(int)operator-=operator-
  (C++20)** — advances or decrements the iterator (public member function)

### Non-member functions

- **operator==operator<=> (C++20)** — compares the distances to the end
  (function template)
- **operator==(std::default_sentinel) (C++20)** — checks if the distance to the
  end is equal to `​0​` (function template)
- **operator+ (C++20)** — advances the iterator (function template)
- **operator- (C++20)** — computes the distance between two iterator adaptors
  (function template)
- **operator-(std::default_sentinel_t) (C++20)** — computes the signed distance
  to the end (function template)
- **iter_move (C++20)** — casts the result of dereferencing the underlying
  iterator to its associated rvalue reference type (function)
- **iter_swap (C++20)** — swaps the objects pointed to by two underlying
  iterators (function template)

### Helper classes

- **std::iterator_traits<std::counted_iterator> (C++20)** — provides uniform
  interface to the properties of the `std::counted_iterator` type (class
  template specialization)

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <string>
#include <vector>

using std::operator""s;

void print(auto const remark, auto const& v)
{
    const auto size = std::ssize(v);
    std::cout << remark << '[' << size << "] { ";
    for (auto it = std::counted_iterator{std::cbegin(v), size};
         it != std::default_sentinel; ++it)
        std::cout << *it << (it.count() > 1 ? ", " : " ");
    std::cout << "}\n";
}

int main()
{
    const auto src = {"Arcturus"s, "Betelgeuse"s, "Canopus"s, "Deneb"s, "Elnath"s};
    print("src", src);
    std::vector<decltype(src)::value_type> dst;
    std::ranges::copy(std::counted_iterator{src.begin(), 3},
                      std::default_sentinel,
                      std::back_inserter(dst));
    print("dst", dst);
}
```

Output:

```text
src[5] { Arcturus, Betelgeuse, Canopus, Deneb, Elnath }
dst[3] { Arcturus, Betelgeuse, Canopus }
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2259R1 | C++20 | member typedefs are not provided `std::incrementable_traits`
      is specialized for `counted_iterator` | member typedefs are added to
      account for `iterator_traits` fix redundant `std::incrementable_traits`
      specialization is removed

### See also

- **default_sentinel_t (C++20)** — default sentinel for use with iterators that
  know the bound of their range (class)
- **views::counted (C++20)** — creates a subrange from an iterator and a count
  (customization point object)
- **ranges::take_viewviews::take (C++20)** — a `view` consisting of the first N
  elements of another `view` (class template) (range adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/iterator/counted_iterator*
