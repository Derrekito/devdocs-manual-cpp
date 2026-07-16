# std::istreambuf_iterator

```cpp
template< class CharT, class Traits = std::char_traits<CharT> >
class istreambuf_iterator
    : public std::iterator<std::input_iterator_tag,
                           CharT, typename Traits::off_type,
                           /* unspecified */, CharT>  // (until C++17)
template< class CharT, class Traits = std::char_traits<CharT> >
class istreambuf_iterator;  // (since C++17)
```

`std::istreambuf_iterator` is a single-pass input iterator that reads successive
characters from the `std::basic_streambuf` object for which it was constructed.

The default-constructed `std::istreambuf_iterator` is known as the
*end-of-stream* iterator. When a valid `std::istreambuf_iterator` reaches the
end of the underlying stream, it becomes equal to the end-of-stream iterator.
Dereferencing or incrementing it further invokes undefined behavior.

`std::istreambuf_iterator` has a trivial copy constructor, a constexpr default
constructor, and a trivial destructor.
*(since C++11)*

### Member types

- **`iterator_category`** — std::input_iterator_tag
- **`value_type`** — CharT
- **`difference_type`** — typename Traits::off_type
- **`pointer`** — /* unspecified */
- **`reference`** — CharT
- **`char_type`** — `CharT`
- **`traits_type`** — `Traits`
- **`int_type`** — typename Traits::int_type
- **`streambuf_type`** — std::basic_streambuf<CharT, Traits>
- **`istream_type`** — std::basic_istream<CharT, Traits>
- **/* proxy */** — Implementation-defined class type. The name `proxy` is for
  exposition only. A `proxy` object holds a `char_type` character and a
  `streambuf_type*` pointer. Dereferencing a `proxy` object with `operator*`
  yields the stored character.

Member types `iterator_category`, `value_type`, `difference_type`, `pointer` and
`reference` are required to be obtained by inheriting from
std::iterator<std::input_iterator_tag, CharT, typename Traits::off_type, /*
unspecified */, CharT>.
*(until C++17)*

The member type `pointer` is usually `CharT*` (see below).

### Member functions

- **(constructor)** — constructs a new `istreambuf_iterator` (public member
  function)
- **(destructor) (implicitly declared)** — destructs an `istreambuf_iterator`
  (public member function)
- **operator*** — obtains a copy of the current character (public member
  function)
- **operator++operator++(int)** — advances the iterator (public member function)
- **equal** — tests if both `istreambuf_iterator`s are end-of-stream or if both
  are valid (public member function)

### Non-member functions

- **operator==operator!= (removed in C++20)** — compares two
  `istreambuf_iterator`s (function template)

### Notes

The resolution of LWG issue 659 introduced operator->. It is expected that given
an `std::istreambuf_iterator` `i`, the expressions `(*i).m` and `i->m` have the
same effect.

However, the resolution does not provide a formal specification of its behavior.
Thus it is implemented differently, including returning `nullptr`, returning the
address of a temporary, or does even provide the member at all. Its intended
behavior can hardly be achieved, and it is removed by the resolution of LWG
issue 2790.

The resolution of LWG issue 659 also made the member type `pointer` unspecified
in order to allow `operator->` to return a proxy. This is to allow `operator->`
to compile when `CharT` is not a class type.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <sstream>
#include <string>

int main()
{
    // typical use case: an input stream represented as a pair of iterators
    std::istringstream in{"Hello, world"};
    std::istreambuf_iterator<char> it{in}, end;
    std::string ss{it, end};
    std::cout << "ss has " << ss.size() << " bytes; "
                 "it holds \"" << ss << "\"\n";

    // demonstration of the single-pass nature
    std::istringstream s{"abc"};
    std::istreambuf_iterator<char> i1{s}, i2{s};
    std::cout << "i1 returns '" << *i1 << "'\n"
                 "i2 returns '" << *i2 << "'\n";

    ++i1;
    std::cout << "after incrementing i1, but not i2:\n"
                 "i1 returns '" << *i1 << "'\n"
                 "i2 returns '" << *i2 << "'\n";

    ++i2;
    std::cout << "after incrementing i2, but not i1:\n"
                 "i1 returns '" << *i1 << "'\n"
                 "i2 returns '" << *i2 << "'\n";
}
```

Output:

```text
ss has 12 bytes; it holds "Hello, world"
i1 returns 'a'
i2 returns 'a'
after incrementing i1, but not i2:
i1 returns 'b'
i2 returns 'b'
after incrementing i2, but not i1:
i1 returns 'c'
i2 returns 'c'
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 659 | C++98 | 1. `std::istreambuf_iterator` did not have operator-> 2. the
      member type `pointer` was specified as `CharT*` | 1. added 2. made
      unspecified
  LWG 2790 | C++98 | the operator-> added by LWG issue 659 was not useful |
      removed

### See also

- **ostreambuf_iterator** — output iterator that writes to
  `std::basic_streambuf` (class template)
- **istream_iterator** — input iterator that reads from `std::basic_istream`
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/istreambuf_iterator*
