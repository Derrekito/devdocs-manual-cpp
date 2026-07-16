# std::basic_regex

```cpp
template<
    class CharT,
    class Traits = std::regex_traits<CharT>
> class basic_regex;  // (since C++11)
```

The class template `basic_regex` provides a general framework for holding
regular expressions.

Several typedefs for common character types are provided:

- **`std::regex`** — std::basic_regex<char>
- **`std::wregex`** — std::basic_regex<wchar_t>

### Member types

- **`value_type`** — `CharT`
- **`traits_type`** — `Traits`
- **`string_type`** — `Traits::string_type`
- **`locale_type`** — `Traits::locale_type`
- **`flag_type`** — `std::regex_constants::syntax_option_type`

### Member functions

- **(constructor)** — constructs the regex object (public member function)
- **(destructor)** — destructs the regex object (public member function)
- **operator=** — assigns the contents (public member function)
- **assign** — assigns the contents (public member function)

**Observers**

- **mark_count** — returns the number of marked sub-expressions within the
  regular expression (public member function)
- **flags** — returns the syntax flags (public member function)

**Locale**

- **getloc** — get locale information (public member function)
- **imbue** — set locale information (public member function)

**Modifiers**

- **swap** — swaps the contents (public member function)

**Constants**

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

The member constants in `basic_regex` are duplicates of the `syntax_option_type`
constants defined in the namespace `std::regex_constants`.

### Non-member functions

- **std::swap(std::basic_regex) (C++11)** — specializes the `std::swap`
  algorithm (function template)

### Deduction guides(since C++17)

---
*Source: https://en.cppreference.com/w/cpp/regex/basic_regex*
