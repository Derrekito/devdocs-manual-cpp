# std::regex_search

```cpp
template< class BidirIt,
          class Alloc, class CharT, class Traits >
bool regex_search( BidirIt first, BidirIt last,
                   std::match_results<BidirIt,Alloc>& m,
                   const std::basic_regex<CharT,Traits>& e,
                   std::regex_constants::match_flag_type flags =
                       std::regex_constants::match_default );  // (1) (since C++11)
template< class CharT, class Alloc, class Traits >
bool regex_search( const CharT* str,
                   std::match_results<const CharT*,Alloc>& m,
                   const std::basic_regex<CharT,Traits>& e,
                   std::regex_constants::match_flag_type flags =
                       std::regex_constants::match_default );  // (2) (since C++11)
template< class STraits, class SAlloc,
          class Alloc, class CharT, class Traits >
bool regex_search( const std::basic_string<CharT,STraits,SAlloc>& s,
                   std::match_results<
                       typename std::basic_string<CharT,STraits,SAlloc>::const_iterator,
                       Alloc
                   >& m,
                   const std::basic_regex<CharT, Traits>& e,
                   std::regex_constants::match_flag_type flags =
                       std::regex_constants::match_default );  // (3) (since C++11)
template< class BidirIt,
          class CharT, class Traits >
bool regex_search( BidirIt first, BidirIt last,
                   const std::basic_regex<CharT,Traits>& e,
                   std::regex_constants::match_flag_type flags =
                       std::regex_constants::match_default );  // (4) (since C++11)
template< class CharT, class Traits >
bool regex_search( const CharT* str,
                   const std::basic_regex<CharT,Traits>& e,
                   std::regex_constants::match_flag_type flags =
                       std::regex_constants::match_default );  // (5) (since C++11)
template< class STraits, class SAlloc,
          class CharT, class Traits >
bool regex_search( const std::basic_string<CharT,STraits,SAlloc>& s,
                   const std::basic_regex<CharT,Traits>& e,
                   std::regex_constants::match_flag_type flags =
                       std::regex_constants::match_default );  // (6) (since C++11)
template< class STraits, class SAlloc,
          class Alloc, class CharT, class Traits >
bool regex_search( const std::basic_string<CharT,STraits,SAlloc>&&,
                   std::match_results<
                       typename std::basic_string<CharT,STraits,SAlloc>::const_iterator,
                       Alloc
                   >&,
                   const std::basic_regex<CharT, Traits>&,
                   std::regex_constants::match_flag_type flags =
                       std::regex_constants::match_default ) = delete;  // (7) (since C++11)
```

Determines if there is a match between the regular expression `e` and some
subsequence in the target character sequence.

1) Analyzes generic range `[``first``,``last``)`. Match results are returned in
   `m`.

2) Analyzes a null-terminated string pointed to by `str`. Match results are
   returned in `m`.

3) Analyzes a string `s`. Match results are returned in `m`.

4-6) Equivalent to (1-3), just omits the match results.

7) The overload (3) is prohibited from accepting temporary strings, otherwise
   this function populates `match_results` `m` with string iterators that become
   invalid immediately.

`regex_search` will successfully match any subsequence of the given sequence,
whereas `std::regex_match` will only return `true` if the regular expression
matches the *entire* sequence.

### Parameters

- **first, last** — a range identifying the target character sequence
- **str** — a pointer to a null-terminated target character sequence
- **s** — a string identifying target character sequence
- **e** — the `std::regex` that should be applied to the target character
  sequence
- **m** — the match results
- **flags** — `std::regex_constants::match_flag_type` governing search behavior

**Type requirements**

**-`BidirIt` must meet the requirements of LegacyBidirectionalIterator.**

**-`Alloc` must meet the requirements of Allocator.**

### Return value

Returns `true` if a match exists, `false` otherwise. In either case, the object
`m` is updated, as follows:

If the match does not exist:

**`m.ready() == true`**

**`m.empty() == true`**

**`m.size() == 0`**

If the match exists:

- **`m.ready()`** — `true`
- **`m.empty()`** — `false`
- **`m.size()`** — number of marked subexpressions plus 1, that is, `1 +
  e.mark_count()`
- **`m.prefix().first`** — `first`
- **`m.prefix().second`** — `m[0].first`
- **`m.prefix().matched`** — `m.prefix().first != m.prefix().second`
- **`m.suffix().first`** — `m[0].second`
- **`m.suffix().second`** — `last`
- **`m.suffix().matched`** — `m.suffix().first != m.suffix().second`
- **`m[0].first`** — the start of the matching sequence
- **`m[0].second`** — the end of the matching sequence
- **`m[0].matched`** — `true`
- **`m[n].first`** — the start of the sequence that matched marked
  sub-expression n, or `last` if the subexpression did not participate in the
  match
- **`m[n].second`** — the end of the sequence that matched marked sub-expression
  n, or `last` if the subexpression did not participate in the match
- **`m[n].matched`** — `true` if sub-expression n participated in the match,
  `false` otherwise

### Notes

In order to examine all matches within the target sequence, `std::regex_search`
may be called in a loop, restarting each time from `m[0].second` of the previous
call. `std::regex_iterator` offers an easy interface to this iteration.

### Example

```cpp
#include <cstddef>
#include <iostream>
#include <regex>
#include <string>

int main()
{
    std::string lines[] = {"Roses are #ff0000",
                           "violets are #0000ff",
                           "all of my base are belong to you"};

    std::regex color_regex("#([a-f0-9]{2})"
                            "([a-f0-9]{2})"
                            "([a-f0-9]{2})");

    // simple match
    for (const auto& line : lines)
        std::cout << line << ": " << std::boolalpha
                  << std::regex_search(line, color_regex) << '\n';
    std::cout << '\n';

    // show contents of marked subexpressions within each match
    std::smatch color_match;
    for (const auto& line : lines)
        if (std::regex_search(line, color_match, color_regex))
        {
            std::cout << "matches for '" << line << "'\n";
            std::cout << "Prefix: '" << color_match.prefix() << "'\n";
            for (std::size_t i = 0; i < color_match.size(); ++i)
                std::cout << i << ": " << color_match[i] << '\n';
            std::cout << "Suffix: '" << color_match.suffix() << "\'\n\n";
        }

    // repeated search (see also std::regex_iterator)
    std::string log(R"(
        Speed:        366
        Mass:        35
        Speed:        378
        Mass:        32
        Speed:        400
        Mass:        30)");
    std::regex r(R"(Speed:\t\d*)");
    for (std::smatch sm; regex_search(log, sm, r);)
    {
        std::cout << sm.str() << '\n';
        log = sm.suffix();
    }

    // C-style string demo
    std::cmatch cm;
    if (std::regex_search("this is a test", cm, std::regex("test")))
        std::cout << "\nFound " << cm[0] << " at position "
                  << cm.prefix().length() << '\n';
}
```

Output:

```text
Roses are #ff0000: true
violets are #0000ff: true
all of my base are belong to you: false

matches for 'Roses are #ff0000'
Prefix: 'Roses are '
0: #ff0000
1: ff
2: 00
3: 00
Suffix: ''

matches for 'violets are #0000ff'
Prefix: 'violets are '
0: #0000ff
1: 00
2: 00
3: ff
Suffix: ''

Speed:        366
Speed:        378
Speed:        400

Found test at position 10
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2329 | C++11 | `basic_string` rvalues were accepted, which was likely to
      result in dangling iterators | rejected via a deleted overload

### See also

- **basic_regex (C++11)** — regular expression object (class template)
- **match_results (C++11)** — identifies one regular expression match, including
  all sub-expression matches (class template)
- **regex_match (C++11)** — attempts to match a regular expression to an entire
  character sequence (function template)

---
*Source: https://en.cppreference.com/w/cpp/regex/regex_search*
