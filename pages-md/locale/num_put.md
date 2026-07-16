# std::num_put

```cpp
template<
    class CharT,
    class OutputIt = std::ostreambuf_iterator<CharT>
> class num_put;
```

Class `std::num_put` encapsulates the rules for formatting numeric values as
strings. Specifically, the types bool, long, unsigned long, `long long`,
`unsigned long long`(since C++11), double, long double, void*, and of all types
implicitly convertible to these (such as int or float) are supported. The
standard formatting output operators (such as `cout << n;`) use the
`std::num_put` facet of the I/O stream's locale to generate text representation
of numbers.

If a `std::num_put` specialization is not guaranteed to be provided by the
standard library (see below), the behaviors of its `put()` and `do_put()` are
not guaranteed as specified.

### Specializations

The standard library is guaranteed to provide the following specializations
(they are required to be implemented by any locale object):

- **std::num_put<char>** — creates narrow string representations of numbers
- **std::num_put<wchar_t>** — creates wide string representations of numbers

In addition, the standard library is also guaranteed to provide every
specialization that satisfies the following type requirements:

- `CharT` is one of char, wchar_t, and any other implementation-defined
  character type that meets the requirements for a character on which any of the
  iostream components can be instantiated; and
- `OutputIt` must meet the requirements of LegacyOutputIterator.

### Member types

- **`char_type`** — `CharT`
- **`iter_type`** — `OutputIt`

### Member functions

- **(constructor)** — constructs a new `num_put` facet (public member function)
- **put** — invokes `do_put` (public member function)

### Protected member functions

- **(destructor)** — destructs a `num_put` facet (protected member function)
- **do_put [virtual]** — formats a number and writes to output stream (virtual
  protected member function)

### Member objects

- **static std::locale::id id** — *id* of the locale (public member object)

### Example

```cpp
#include <iostream>
#include <iterator>
#include <locale>
#include <string>

int main()
{
    double n = 1234567.89;
    std::cout.imbue(std::locale("de_DE.UTF-8"));
    std::cout << "Direct conversion to string:\n"
              << std::to_string(n) << '\n'
              << "Output using a german locale:\n"
              << std::fixed << n << '\n'
              << "Output using an american locale:\n";

    // use the facet directly
    std::cout.imbue(std::locale("en_US.UTF-8"));
    auto& f = std::use_facet<std::num_put<char>>(std::cout.getloc());
    f.put(std::ostreambuf_iterator<char>(std::cout), std::cout, ' ', n);
    std::cout << '\n';
}
```

Possible output:

```text
Direct conversion to string:
1234567.890000
Output using a german locale:
1.234.567,890000
Output using an american locale:
1,234,567.890000
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 427 | C++98 | `num_put` could accept any `CharT` that meets the
      requirements for a character on which any of the iostream components can
      be instantiated | only guarantees to accept char and wchar_t, other types
      are implementation-defined

### See also

- **numpunct** — defines numeric punctuation rules (class template)
- **num_get** — parses numeric values from an input character sequence (class
  template)
- **to_string (C++11)** — converts an integral or floating-point value to
  `string` (function)
- **to_wstring (C++11)** — converts an integral or floating-point value to
  `wstring` (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/num_put*
