# std::istream_iterator

```cpp
template< class T,
          class CharT = char,
          class Traits = std::char_traits<CharT>,
          class Distance = std::ptrdiff_t >
class istream_iterator
    : public std::iterator<std::input_iterator_tag, T, Distance, const T*, const T&>  // (until C++17)
template< class T,
          class CharT = char,
          class Traits = std::char_traits<CharT>,
          class Distance = std::ptrdiff_t >
class istream_iterator;  // (since C++17)
```

`std::istream_iterator` is a single-pass input iterator that reads successive
objects of type `T` from the `std::basic_istream` object for which it was
constructed, by calling the appropriate operator>>. The actual read operation is
performed when the iterator is incremented, not when it is dereferenced. The
first object is read when the iterator is constructed. Dereferencing only
returns a copy of the most recently read object.

The default-constructed `std::istream_iterator` is known as the *end-of-stream*
iterator. When a valid `std::istream_iterator` reaches the end of the underlying
stream, it becomes equal to the end-of-stream iterator. Dereferencing or
incrementing it further invokes undefined behavior. An end-of-stream iterator
remains in the end-of-stream state even if the underlying stream changes state.
Absent a reassignment, it cannot become a non-end-of-stream iterator anymore.

A typical implementation of `std::istream_iterator` holds two data members: a
pointer to the associated `std::basic_istream` object and the most recently read
value of type `T`.

`T` must meet the DefaultConstructible, CopyConstructible, and CopyAssignable
requirements.

### Member types

- **`iterator_category`** — std::input_iterator_tag
- **`value_type`** — T
- **`difference_type`** — Distance
- **`pointer`** — const T*
- **`reference`** — const T&
- **`char_type`** — `CharT`
- **`traits_type`** — `Traits`
- **`istream_type`** — std::basic_istream<CharT, Traits>

Member types `iterator_category`, `value_type`, `difference_type`, `pointer` and
`reference` are required to be obtained by inheriting from
std::iterator<std::input_iterator_tag, T, Distance, const T*, const T&>.
*(until C++17)*

### Member functions

- **(constructor)** — constructs a new `istream_iterator` (public member
  function)
- **(destructor)** — destructs an `istream_iterator`, including the cached value
  (public member function)
- **operator*operator->** — returns the current element (public member function)
- **operator++operator++(int)** — advances the iterator (public member function)

### Non-member functions

- **operator==operator!= (removed in C++20)** — compares two `istream_iterator`s
  (function template)

### Notes

When reading characters, `std::istream_iterator` skips whitespace by default
(unless disabled with `std::noskipws` or equivalent), while
`std::istreambuf_iterator` does not. In addition, `std::istreambuf_iterator` is
more efficient, since it avoids the overhead of constructing and destructing the
sentry object once per character.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <numeric>
#include <sstream>

int main()
{
    std::istringstream str("0.1 0.2 0.3 0.4");
    std::partial_sum(std::istream_iterator<double>(str),
                     std::istream_iterator<double>(),
                     std::ostream_iterator<double>(std::cout, " "));

    std::istringstream str2("1 3 5 7 8 9 10");
    auto it = std::find_if(std::istream_iterator<int>(str2),
                           std::istream_iterator<int>(),
                           [](int i){ return i % 2 == 0; });

    if (it != std::istream_iterator<int>())
        std::cout << "\nThe first even number is " << *it << ".\n";
    //" 9 10" left in the stream
}
```

Output:

```text
0.1 0.3 0.6 1
The first even number is 8.
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0738R2 | C++98 | the first read might be deferred to the first dereference |
      always performed in the constructor

### See also

- **ostream_iterator** — output iterator that writes to `std::basic_ostream`
  (class template)
- **istreambuf_iterator** — input iterator that reads from
  `std::basic_streambuf` (class template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/istream_iterator*
