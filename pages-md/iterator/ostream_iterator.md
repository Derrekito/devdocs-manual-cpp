# std::ostream_iterator

```cpp
template< class T,
          class CharT = char,
          class Traits = std::char_traits<CharT> >
class ostream_iterator
    : public std::iterator<std::output_iterator_tag, void, void, void, void>  // (until C++17)
template< class T,
          class CharT = char,
          class Traits = std::char_traits<CharT> >
class ostream_iterator;  // (since C++17)
```

`std::ostream_iterator` is a single-pass LegacyOutputIterator that writes
successive objects of type `T` into the `std::basic_ostream` object for which it
was constructed, using `operator<<`. Optional delimiter string is written to the
output stream after every write operation. The write operation is performed when
the iterator (whether dereferenced or not) is assigned to. Incrementing the
`std::ostream_iterator` is a no-op.

In a typical implementation, the only data members of `std::ostream_iterator`
are a pointer to the associated `std::basic_ostream` and a pointer to the first
character in the delimiter string.

When writing characters, `std::ostreambuf_iterator` is more efficient, since it
avoids the overhead of constructing and destructing the sentry object once per
character.

### Member types

- **`iterator_category`** — std::output_iterator_tag
- **`value_type`** — void
- **`difference_type`** — void (until C++20) `std::ptrdiff_t` (since C++20)
- **void** — (until C++20)
- **`std::ptrdiff_t`** — (since C++20)
- **`pointer`** — void
- **`reference`** — void
- **`char_type`** — `CharT`
- **`traits_type`** — `Traits`
- **`ostream_type`** — std::basic_ostream<CharT, Traits>

Member types `iterator_category`, `value_type`, `difference_type`, `pointer` and
`reference` are required to be obtained by inheriting from
std::iterator<std::output_iterator_tag, void, void, void, void>.
*(until C++17)*

### Member functions

- **(constructor)** — constructs a new ostream_iterator (public member function)
- **(destructor)** — destructs an `ostream_iterator` (public member function)
- **operator=** — writes an object to the associated output sequence (public
  member function)
- **operator*** — no-op (public member function)
- **operator++operator++(int)** — no-op (public member function)

### Example

```cpp
#include <iostream>
#include <iterator>
#include <numeric>
#include <sstream>

int main()
{
    std::istringstream str("0.11 0.22 0.33 0.44");

    std::partial_sum(std::istream_iterator<double>(str),
                     std::istream_iterator<double>(),
                     std::ostream_iterator<double>(std::cout, ", "));
    std::cout << '\n';
}
```

Output:

```text
0.11, 0.33, 0.66, 1.1,
```

### See also

- **ostreambuf_iterator** — output iterator that writes to
  `std::basic_streambuf` (class template)
- **istream_iterator** — input iterator that reads from `std::basic_istream`
  (class template)
- **std::experimental::ostream_joiner (library fundamentals TS v2)** — an output
  iterator that writes successive elements into an output stream, separating
  adjacent elements with a delimiter (class template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/ostream_iterator*
