# std::regex_iterator<BidirIt,CharT,Traits>::operator==,operator!=

```cpp
bool operator==( const regex_iterator& rhs ) const;  // (1) (since C++11)
bool operator!=( const regex_iterator& rhs ) const;  // (2) (since C++11) (until C++20)
```

Compares two `regex_iterator`s.

The `!=` operator is synthesized from `operator==`.
*(since C++20)*

### Parameters

- **rhs** — a `regex_iterator` to compare to

### Return value

For the sake of exposition, assume that `regex_iterator` contains the following
members:

- `BidirIt begin`;
- `BidirIt end`;
- `const regex_type *pregex;`
- `std::regex_constants::match_flag_type flags;`
- `std::match_results<BidirIt> match;`

1) Returns `true` if `*this` and `rhs` are both end-of-sequence iterators, or if
all of the following conditions are true:

- `begin == rhs.begin`
- `end == rhs.end`
- `pregex == rhs.pregex`
- `flags == rhs.flags`
- `match[0] == rhs.match[0]`

2) Returns `!(*this == rhs)`.

### Example

---
*Source: https://en.cppreference.com/w/cpp/regex/regex_iterator/operator_cmp*
