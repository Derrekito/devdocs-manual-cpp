# std::regex_iterator<BidirIt,CharT,Traits>::regex_iterator

```cpp
regex_iterator();  // (1) (since C++11)
regex_iterator( BidirIt a, BidirIt b,
                const regex_type& re,
                std::regex_constants::match_flag_type m =
                    std::regex_constants::match_default );  // (2) (since C++11)
regex_iterator( const regex_iterator& );  // (3) (since C++11)
regex_iterator( BidirIt, BidirIt,
                const regex_type&&,
                std::regex_constants::match_flag_type =
                    std::regex_constants::match_default ) = delete;  // (4) (since C++11)
```

Constructs a new `regex_iterator`:

1) Default constructor. Constructs an end-of-sequence iterator.

2) Constructs a `regex_iterator` from the sequence of characters
   `[``a``,``b``)`, the regular expression `re`, and a flag `m` that governs
   matching behavior. This constructor performs an initial call to
   `std::regex_search` with this data. If the result of this initial call is
   `false`, `*this` is set to an end-of-sequence iterator.

3) Copies a `regex_iterator`.

4) The overload (2) is not allowed to be called with a temporary regex, since
   the returned iterator would be immediately invalidated.

### Parameters

- **a** — LegacyBidirectionalIterator to the beginning of the target character
  sequence
- **b** — LegacyBidirectionalIterator to the end of the target character
  sequence
- **re** — regular expression used to search the target character sequence
- **m** — flags that govern the behavior of `re`

### Example

```cpp
#include <iostream>
#include <regex>
#include <string_view>

int main()
{
    constexpr std::string_view str{R"(
        #ONE: *p = &Mass;
        #Two: MOV %rd, 42
    )"};
    const std::regex re("[a-w]");

    // create regex_iterator, overload (2)
    auto it = std::regex_iterator<std::string_view::iterator>
    {
        str.cbegin(), str.cend(),
        re // re is lvalue; if an immediate expression was used
           // instead, e.g. std::regex{"[a-z]"}, this would
           // produce an error since overload (4) is deleted
    };

    for (decltype(it) last /* overload (1) */; it != last; ++it)
        std::cout << (*it).str();
    std::cout << '\n';
}
```

Output:

```text
password
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2332 | C++11 | a `regex_iterator` constructed from a temporary
      `basic_regex` became invalid immediately | such construction is disallowed
      via a deleted overload

---
*Source: https://en.cppreference.com/w/cpp/regex/regex_iterator/regex_iterator*
