# std::basic_regex constants

```cpp
static constexpr std::regex_constants::syntax_option_type icase =
    std::regex_constants::icase;
static constexpr std::regex_constants::syntax_option_type nosubs =
    std::regex_constants::nosubs;
static constexpr std::regex_constants::syntax_option_type optimize =
    std::regex_constants::optimize;
static constexpr std::regex_constants::syntax_option_type collate =
    std::regex_constants::collate;
static constexpr std::regex_constants::syntax_option_type ECMAScript =
    std::regex_constants::ECMAScript;
static constexpr std::regex_constants::syntax_option_type basic =
    std::regex_constants::basic;
static constexpr std::regex_constants::syntax_option_type extended =
    std::regex_constants::extended;
static constexpr std::regex_constants::syntax_option_type awk =
    std::regex_constants::awk;
static constexpr std::regex_constants::syntax_option_type grep =
    std::regex_constants::grep;
static constexpr std::regex_constants::syntax_option_type egrep =
    std::regex_constants::egrep;
static constexpr std::regex_constants::syntax_option_type multiline =
    std::regex_constants::multiline;  // (since C++17)
```

`std::basic_regex` defines several constants that govern general regex matching
syntax.

These constants are duplicated from `std::regex_constants`:

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

### See also

- **syntax_option_type (C++11)** — general options controlling regex behavior
  (typedef)

---
*Source: https://en.cppreference.com/w/cpp/regex/basic_regex/constants*
