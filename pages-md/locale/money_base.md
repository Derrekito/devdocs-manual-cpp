# std::money_base

```cpp
class money_base;
```

The class `std::money_base` provides constants which are inherited and used by
the `std::moneypunct`, `std::money_get` and `std::money_put` facets.

### Member types

- **`enum part { none, space, symbol, sign, value };`** — unscoped enumeration
  type
- **`struct pattern { char field[4]; };`** — the monetary format type

- **`none`** — whitespace is permitted but not required except in the last
  position, where whitespace is not permitted
- **`space`** — one or more whitespace characters are required
- **`symbol`** — the sequence of characters returned by moneypunct::curr_symbol
  is required
- **`sign`** — the first of the characters returned by moneypunct::positive_sign
  or moneypunct::negative_sign is required
- **`value`** — the absolute numeric monetary value is required

### Notes

The monetary format is an array of four `char`s convertible to
`std::money_base::part`. In that sequence, each of `symbol`, `sign`, and `value`
appears exactly once, and either `space` or `none` appears in the remaining
position. The value `none`, if present, is not first; the value `space`, if
present, is neither first nor last.

The default format, returned by the standard specializations of
`std::moneypunct` is `{symbol, sign, none, value}`.

### See also

- **moneypunct** — defines monetary formatting parameters used by
  `std::money_get` and `std::money_put` (class template)
- **money_get** — parses and constructs a monetary value from an input character
  sequence (class template)
- **money_put** — formats a monetary value for output as a character sequence
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/locale/money_base*
