# std::regex_token_iterator<BidirIt,CharT,Traits>::regex_token_iterator

```cpp
regex_token_iterator();  // (1) (since C++11)
regex_token_iterator( BidirIt a, BidirIt b,
                      const regex_type& re,
                      int submatch = 0,
                      std::regex_constants::match_flag_type m =
                          std::regex_constants::match_default );  // (2) (since C++11)
regex_token_iterator( BidirIt a, BidirIt b,
                      const regex_type& re,
                      const std::vector<int>& submatches,
                      std::regex_constants::match_flag_type m =
                          std::regex_constants::match_default );  // (3) (since C++11)
regex_token_iterator( BidirIt a, BidirIt b,
                      const regex_type& re,
                      std::initializer_list<int> submatches,
                      std::regex_constants::match_flag_type m =
                          std::regex_constants::match_default );  // (4) (since C++11)
template< std::size_t N >
regex_token_iterator( BidirIt a, BidirIt b,
                      const regex_type& re,
                      const int (&submatches)[N],
                      std::regex_constants::match_flag_type m =
                          std::regex_constants::match_default );  // (5) (since C++11)
regex_token_iterator( const regex_token_iterator& other );  // (6) (since C++11)
regex_token_iterator( BidirIt a, BidirIt b,
                      const regex_type&& re,
                      int submatch = 0,
                      std::regex_constants::match_flag_type m =
                          std::regex_constants::match_default ) = delete;  // (7) (since C++11)
regex_token_iterator( BidirIt a, BidirIt b,
                      const regex_type&& re,
                      const std::vector<int>& submatches,
                      std::regex_constants::match_flag_type m =
                          std::regex_constants::match_default ) = delete;  // (8) (since C++11)
regex_token_iterator( BidirIt a, BidirIt b,
                      const regex_type&& re,
                      std::initializer_list<int> submatches,
                      std::regex_constants::match_flag_type m =
                          std::regex_constants::match_default ) = delete;  // (9) (since C++11)
template< std::size_t N >
regex_token_iterator( BidirIt a, BidirIt b,
                      const regex_type&& re,
                      const int (&submatches)[N],
                      std::regex_constants::match_flag_type m =
                          std::regex_constants::match_default ) = delete;  // (10) (since C++11)
```

Constructs a new `regex_token_iterator`:

1) Default constructor. Constructs the end-of-sequence iterator.
2-5) First, copies the list of the requested submatch out of the `submatches` or
`submatch` argument into the member list stored in the iterator and constructs
the member `std::regex_iterator` by passing `a`, `b`, `re`, and `m` to its
four-argument constructor (that constructor performs the initial call to
`std::regex_search`) and sets the internal counter of `submatches` to zero.

- If, after construction, the member `regex_iterator` is not an end-of-sequence
  iterator, sets the member pointer to the address of the current
  `std::sub_match`.
- Otherwise (if the member `regex_iterator` is an end-of-sequence iterator), but
  the value `-1` is one of the values in `submatches`/`submatch`, turns `*this`
  into a *suffix iterator* pointing at the range `[``a``,``b``)` (the entire
  string is the non-matched suffix).
- Otherwise (if `-1` is not in the list of `submatches`), turns `*this` into the
  end-of-sequence iterator.

The behavior is undefined if any value in `submatches` is less than `-1`.

6) Copy constructor: performs member-wise copy (including making a copy of the
   member `regex_iterator` and the member pointer to current `sub_match`).

7-10) The overloads (2-5) are prohibited from being called with a temporary
   regex since otherwise the returned iterator would be immediately invalidated.

### Parameters

- **a** — LegacyBidirectionalIterator to the beginning of the target character
  sequence
- **b** — LegacyBidirectionalIterator to the end of the target character
  sequence
- **re** — regular expression used to search the target character sequence
- **submatch** — the index of the submatch that should be returned. "0"
  represents the entire match, and "-1" represents the parts that are not
  matched (e.g, the stuff between matches)
- **submatches** — the sequence of submatch indices that should be iterated over
  within each match, may include the special value `-1` for the non-matched
  fragments
- **m** — flags that govern the behavior of `re`

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2332 | C++11 | a `regex_token_iterator` constructed from a temporary
      `basic_regex` became invalid immediately | such construction is disallowed
      via deleted overloads

---
*Source: https://en.cppreference.com/w/cpp/regex/regex_token_iterator/regex_token_iterator*
