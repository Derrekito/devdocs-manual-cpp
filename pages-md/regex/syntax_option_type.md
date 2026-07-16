# std::regex_constants::syntax_option_type

```cpp
using syntax_option_type = /* implementation-defined */;  // (1) (since C++11)
constexpr syntax_option_type icase      = /* unspecified */;
constexpr syntax_option_type nosubs     = /* unspecified */;
constexpr syntax_option_type optimize   = /* unspecified */;
constexpr syntax_option_type collate    = /* unspecified */;
constexpr syntax_option_type ECMAScript = /* unspecified */;
constexpr syntax_option_type basic      = /* unspecified */;
constexpr syntax_option_type extended   = /* unspecified */;
constexpr syntax_option_type awk        = /* unspecified */;
constexpr syntax_option_type grep       = /* unspecified */;
constexpr syntax_option_type egrep      = /* unspecified */;  // (since C++11) (until C++17)
inline constexpr syntax_option_type icase      = /* unspecified */;
inline constexpr syntax_option_type nosubs     = /* unspecified */;
inline constexpr syntax_option_type optimize   = /* unspecified */;
inline constexpr syntax_option_type collate    = /* unspecified */;
inline constexpr syntax_option_type ECMAScript = /* unspecified */;
inline constexpr syntax_option_type basic      = /* unspecified */;
inline constexpr syntax_option_type extended   = /* unspecified */;
inline constexpr syntax_option_type awk        = /* unspecified */;
inline constexpr syntax_option_type grep       = /* unspecified */;
inline constexpr syntax_option_type egrep      = /* unspecified */;  // (since C++17)
inline constexpr syntax_option_type multiline  = /* unspecified */;  // (3) (since C++17)
```

1) The `syntax_option_type` is a BitmaskType that contains options that govern
   how regular expressions behave.

2,3) The possible values (`icase`, `optimize`, etc.) for type (1) are duplicated
   inside `std::basic_regex`.

### Constants

- **`icase`** — Character matching should be performed without regard to case.
- **`nosubs`** — When performing matches, all marked sub-expressions `(expr)`
  are treated as non-marking sub-expressions `(?:expr)`. No matches are stored
  in the supplied `std::regex_match` structure and `mark_count()` is zero.
- **`optimize`** — Instructs the regular expression engine to make matching
  faster, with the potential cost of making construction slower. For example,
  this might mean converting a non-deterministic FSA to a deterministic FSA.
- **`collate`** — Character ranges of the form *"[a-b]"* will be locale
  sensitive.
- **`multiline` (C++17)** — Specifies that `^` shall match the beginning of a
  line and `$` shall match the end of a line, if the ECMAScript engine is
  selected.
- **`ECMAScript`** — Use the Modified ECMAScript regular expression grammar.
- **`basic`** — Use the basic POSIX regular expression grammar (grammar
  documentation).
- **`extended`** — Use the extended POSIX regular expression grammar (grammar
  documentation).
- **`awk`** — Use the regular expression grammar used by the *awk* utility in
  POSIX (grammar documentation).
- **`grep`** — Use the regular expression grammar used by the *grep* utility in
  POSIX. This is effectively the same as the `basic` option with the addition of
  newline `'\n'` as an alternation separator.
- **`egrep`** — Use the regular expression grammar used by the *grep* utility,
  with the *-E* option, in POSIX. This is effectively the same as the `extended`
  option with the addition of newline `'\n'` as an alternation separator in
  addition to `'|'`.

At most one grammar option must be chosen out of `ECMAScript`, `basic`,
`extended`, `awk`, `grep`, `egrep`. If no grammar is chosen, `ECMAScript` is
assumed to be selected. The other options serve as modifiers, such that
`std::regex("meow", std::regex::icase)` is equivalent to `std::regex("meow",
std::regex::ECMAScript|std::regex::icase)`.

### Notes

Because POSIX uses "leftmost longest" matching rule (the longest matching
subsequence is matched, and if there are several such subsequences, the first
one is matched), it is not suitable, for example, for parsing markup languages:
a POSIX regex such as `"<tag[^>]*>.*</tag>"` would match everything from the
first `"<tag"` to the last `"</tag>"`, including every `"</tag>"` and `"<tag>"`
in-between. On the other hand, ECMAScript supports non-greedy matches, and the
ECMAScript regex `"<tag[^>]*>.*?</tag>"` would match only until the first
closing tag.

### Example

Illustrates the difference in the matching algorithm between ECMAScript and
POSIX regular expressions:

```cpp
#include <iostream>
#include <regex>
#include <string>

int main()
{
    std::string str = "zzxayyzz";
    std::regex re1(".*(a|xayy)"); // ECMA
    std::regex re2(".*(a|xayy)", std::regex::extended); // POSIX

    std::cout << "Searching for .*(a|xayy) in zzxayyzz:\n";
    std::smatch m;
    std::regex_search(str, m, re1);
    std::cout << "  ECMA (depth first search) match: " << m[0] << '\n';
    std::regex_search(str, m, re2);
    std::cout << "  POSIX (leftmost longest)  match: " << m[0] << '\n';
}
```

Output:

```text
Searching for .*(a|xayy) in zzxayyzz:
  ECMA (depth first search) match: zzxa
  POSIX (leftmost longest)  match: zzxayy
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2053 | C++11 | the constants were declared static | removed the static
      specifier

### See also

- **basic_regex (C++11)** — regular expression object (class template)

---
*Source: https://en.cppreference.com/w/cpp/regex/syntax_option_type*
