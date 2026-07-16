# std::match_results<BidirIt,Alloc>::format

```cpp
template< class OutputIt >
OutputIt format( OutputIt out,
                 const char_type* fmt_first, const char_type* fmt_last,
                 std::regex_constants::match_flag_type flags =
                     std::regex_constants::format_default ) const;  // (1) (since C++11)
template< class OutputIt, class ST, class SA >
OutputIt format( OutputIt out,
                 const basic_string<char_type,ST,SA>& fmt,
                 std::regex_constants::match_flag_type flags =
                     std::regex_constants::format_default ) const;  // (2) (since C++11)
template< class ST, class SA >
std::basic_string<char_type,ST,SA>
    format( const std::basic_string<char_type,ST,SA>& fmt,
            std::regex_constants::match_flag_type flags =
                std::regex_constants::format_default ) const;  // (3) (since C++11)
string_type format( const char_type* fmt_s,
                    std::regex_constants::match_flag_type flags =
                        std::regex_constants::format_default ) const;  // (4) (since C++11)
```

`format` outputs a format string, replacing any format specifiers or escape
sequences in that string with match data from `*this`.

1) The format character sequence is defined by the range
   `[``fmt_first``,``fmt_last``)`. The resulting character sequence is copied to
   `out`.

2) The format character sequence is defined by the characters in `fmt`. The
   resulting character sequence is copied to `out`.

3,4) The format character sequence is defined by the characters in `fmt` and
   `fmt_s` respectively. The resulting character sequence is copied to a newly
   constructed `std::basic_string`, which is returned.

The `flags` bitmask determines which format specifiers and escape sequences are
recognized.

The behavior of `format` is undefined if `ready() != true`.

### Parameters

- **fmt_begin, fmt_end** — pointers to a range of characters defining the format
  character sequence
- **fmt** — `std::basic_string` defining the format character sequence
- **fmt_s** — pointer to a null-terminated character string defining the format
  character sequence
- **out** — iterator that the resulting character sequence is copied to
- **flags** — `std::regex_constants::match_flag_type` bitmask specifying which
  format specifiers and escape sequences are recognized

**Type requirements**

**-`OutputIt` must meet the requirements of LegacyOutputIterator.**

### Return value

1,2) `out`

3,4) The newly constructed string containing resulting character sequence.

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <iostream>
#include <regex>
#include <string>

int main()
{
    std::string s = "for a good time, call 867-5309";
    std::regex phone_regex("\\d{3}-\\d{4}");
    std::smatch phone_match;

    if (std::regex_search(s, phone_match, phone_regex))
    {
        std::string fmt_s = phone_match.format(
            "$`"   // $` means characters before the match
            "[$&]" // $& means the matched characters
            "$'"); // $' means characters following the match
        std::cout << fmt_s << '\n';
    }
}
```

Output:

```text
for a good time, call [867-5309]
```

### See also

- **regex_replace (C++11)** — replaces occurrences of a regular expression with
  formatted replacement text (function template)
- **match_flag_type (C++11)** — options specific to matching (typedef)

---
*Source: https://en.cppreference.com/w/cpp/regex/match_results/format*
