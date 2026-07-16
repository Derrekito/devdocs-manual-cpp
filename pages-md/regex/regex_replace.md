# std::regex_replace

```cpp
template< class OutputIt, class BidirIt,
          class Traits, class CharT,
          class STraits, class SAlloc >
OutputIt regex_replace( OutputIt out, BidirIt first, BidirIt last,
                        const std::basic_regex<CharT,Traits>& re,
                        const std::basic_string<CharT,STraits,SAlloc>& fmt,
                        std::regex_constants::match_flag_type flags =
                            std::regex_constants::match_default );  // (1) (since C++11)
template< class OutputIt, class BidirIt,
          class Traits, class CharT >
OutputIt regex_replace( OutputIt out, BidirIt first, BidirIt last,
                        const std::basic_regex<CharT,Traits>& re,
                        const CharT* fmt,
                        std::regex_constants::match_flag_type flags =
                            std::regex_constants::match_default );  // (2) (since C++11)
template< class Traits, class CharT,
          class STraits, class SAlloc,
          class FTraits, class FAlloc >
std::basic_string<CharT,STraits,SAlloc>
    regex_replace( const std::basic_string<CharT,STraits,SAlloc>& s,
                   const std::basic_regex<CharT,Traits>& re,
                   const std::basic_string<CharT,FTraits,FAlloc>& fmt,
                   std::regex_constants::match_flag_type flags =
                       std::regex_constants::match_default );  // (3) (since C++11)
template< class Traits, class CharT,
          class STraits, class SAlloc >
std::basic_string<CharT,STraits,SAlloc>
    regex_replace( const std::basic_string<CharT,STraits,SAlloc>& s,
                   const std::basic_regex<CharT,Traits>& re,
                   const CharT* fmt,
                   std::regex_constants::match_flag_type flags =
                       std::regex_constants::match_default );  // (4) (since C++11)
template< class Traits, class CharT,
          class STraits, class SAlloc >
std::basic_string<CharT>
    regex_replace( const CharT* s,
                   const std::basic_regex<CharT,Traits>& re,
                   const std::basic_string<CharT,STraits,SAlloc>& fmt,
                   std::regex_constants::match_flag_type flags =
                       std::regex_constants::match_default );  // (5) (since C++11)
template< class Traits, class CharT >
std::basic_string<CharT>
    regex_replace( const CharT* s,
                   const std::basic_regex<CharT,Traits>& re,
                   const CharT* fmt,
                   std::regex_constants::match_flag_type flags =
                       std::regex_constants::match_default );  // (6) (since C++11)
```

`regex_replace` uses a regular expression to perform substitution on a sequence
of characters:

1) Copies characters in the range `[``first``,``last``)` to `out`, replacing any
   sequences that match `re` with characters formatted by `fmt`. In other words:

- Constructs a `std::regex_iterator` object `i` as if by
  `std::regex_iterator<BidirIt, CharT, traits> i(first, last, re, flags)`, and
  uses it to step through every match of `re` within the sequence
  `[``first``,``last``)`.
- For each such match `m`, copies the non-matched subsequence (`m.prefix()`)
  into `out` as if by `out = std::copy(m.prefix().first, m.prefix().second,
  out)` and then replaces the matched subsequence with the formatted replacement
  string as if by calling `out = m.format(out, fmt, flags)`.
- When no more matches are found, copies the remaining non-matched characters to
  `out` as if by `out = std::copy(last_m.suffix().first, last_m.suffix().second,
  out)` where `last_m` is a copy of the last match found.
- If there are no matches, copies the entire sequence into `out` as-is, by `out
  = std::copy(first, last, out)`.
- If `flags` contains `std::regex_constants::format_no_copy`, the non-matched
  subsequences are not copied into `out`.
- If `flags` contains `std::regex_constants::format_first_only`, only the first
  match is replaced.

2) Same as (1), but the formatted replacement is performed as if by calling `out
   = m.format(out, fmt, fmt + char_traits<CharT>::length(fmt), flags)`.

3,4) Constructs an empty string `result` of type `std::basic_string<CharT, ST,
   SA>` and calls `std::regex_replace(std::back_inserter(result), s.begin(),
   s.end(), re, fmt, flags)`.

5,6) Constructs an empty string `result` of type `std::basic_string<CharT>` and
   calls `std::regex_replace(std::back_inserter(result), s, s +
   std::char_traits<CharT>::length(s), re, fmt, flags)`.

### Parameters

- **first, last** — the input character sequence, represented as a pair of
  iterators
- **s** — the input character sequence, represented as `std::basic_string` or
  character array
- **re** — the `std::basic_regex` that will be matched against the input
  sequence
- **fmt** — the regex replacement format string, exact syntax depends on the
  value of `flags`
- **flags** — the match flags of type `std::regex_constants::match_flag_type`
- **out** — output iterator to store the result of the replacement

**Type requirements**

**-`OutputIt` must meet the requirements of LegacyOutputIterator.**

**-`BidirIt` must meet the requirements of LegacyBidirectionalIterator.**

### Return value

1,2) Returns a copy of the output iterator `out` after all the insertions.

3-6) Returns the string `result` which contains the output.

### Exceptions

May throw `std::regex_error` to indicate an error condition.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <regex>
#include <string>

int main()
{
   std::string text = "Quick brown fox";
   std::regex vowel_re("a|e|i|o|u");

   // write the results to an output iterator
   std::regex_replace(std::ostreambuf_iterator<char>(std::cout),
                      text.begin(), text.end(), vowel_re, "*");

   // construct a string holding the results
   std::cout << '\n' << std::regex_replace(text, vowel_re, "[$&]") << '\n';
}
```

Output:

```text
Q**ck br*wn f*x
Q[u][i]ck br[o]wn f[o]x
```

### See also

- **regex_search (C++11)** — attempts to match a regular expression to any part
  of a character sequence (function template)
- **match_flag_type (C++11)** — options specific to matching (typedef)
- **replace** — replaces specified portion of a string (public member function
  of `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/regex/regex_replace*
