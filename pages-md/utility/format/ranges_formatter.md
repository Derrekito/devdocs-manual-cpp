# std::formatter<*range*>

```cpp
template< ranges::input_range R, class CharT >
    requires (std::format_kind<R> != std::range_format::disabled) &&
              std::formattable<ranges::range_reference_t<R>, CharT>
struct formatter<R, CharT>;  // (since C++23)
Helper templates
template< std::range_format K, ranges::input_range R, class CharT >
struct range-default-formatter;  // (exposition only*)
```

The template specialization of `std::formatter` for the range types allows users
to convert a range to its textual representation as a collection of elements or
a string using formatting functions.

The specialization is derived from `range-default-formatter<std::format_kind<R>,
R, CharT>`.

The specialization is enabled if `R` satisfies `input_range`,
`std::format_kind<R>` is not `std::range_format::disabled`, and
`std::formattable<ranges::range_reference_t<R>, CharT>` is `true`.

### Format specification

The syntax of range-format-spec is:

```text
range-fill-and-align ﻿(optional) width ﻿(optional) n(optional) range-type ﻿(optional) range-underlying-spec ﻿(optional)
```

The syntax is fully described in range format specification.

For specializations of `std::formatter` where `std::format_kind<R>` is either
`std::range_format::string` or `std::range_format::debug_string`, the
format-spec is std-format-spec instead of range-format-spec (which uses
`std::formatter<std::basic_string<CharT>, CharT>` as the underlying formatter).

### Specializations of `range-default-formatter`

- ***range-default-formatter*<std::range_format::sequence> (C++23)** —
  formatting utility for ranges in sequence form (class template specialization)
- ***range-default-formatter*<std::range_format::map> (C++23)** — formatting
  utility for ranges in map form (class template specialization)
- ***range-default-formatter*<std::range_format::set> (C++23)** — formatting
  utility for ranges in set form (class template specialization)
-
  ***range-default-formatter*<std::range_format::string>*range-default-formatter*<std::range_format::debug_string>
  (C++23)** — formatting utility for ranges in string or escaped string form
  (class template specialization)

### Example

### See also

- **formatter (C++20)** — defines formatting rules for a given type (class
  template)
- **range_formatter (C++23)** — class template that helps implementing
  `std::formatter` specializations for range types (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/format/ranges_formatter*
