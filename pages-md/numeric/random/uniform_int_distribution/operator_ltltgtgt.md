# operator<<,>>(std::uniform_int_distribution)

```cpp
template< class CharT, class Traits >
friend std::basic_ostream<CharT,Traits>&
    operator<<( std::basic_ostream<CharT,Traits>& ost,
                const uniform_int_distribution& d );  // (1) (since C++11)
template< class CharT, class Traits >
friend std::basic_istream<CharT,Traits>&
    operator>>( std::basic_istream<CharT,Traits>& ist,
                uniform_int_distribution& d );  // (2) (since C++11)
```

Performs stream input and output operations on pseudo-random number distribution
`d`.

1) Writes a textual representation of the distribution parameters and internal
   state to `ost` as textual representation. The formatting flags and fill
   character of `ost` are unchanged.

2) Restores the distribution parameters and internal state with data read from
   `ist`. The formatting flags of `ist` are unchanged. The data must have been
   written using a stream with the same locale, `CharT` and `Traits` template
   parameters, otherwise the behavior is undefined. If bad input is encountered,
   `ist.setstate(std::ios::failbit)` is called, which may throw
   `std::ios_base::failure`. `d` is unchanged in that case.

These function templates are not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`std::uniform_int_distribution<ResultType>` is an associated class of the
arguments.

### Parameters

- **ost** — output stream to insert the data to
- **ist** — input stream to extract the data from
- **d** — pseudo-random number distribution

### Return value

1) `ost`

2) `ist`

### Exceptions

1) May throw implementation-defined exceptions.

2) May throw `std::ios_base::failure` on bad input.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3519 | C++11 | the form of insertion and extraction operators were
      unspecified (could be hidden friends or out-of-class function templates) |
      specified to be hidden friends

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/uniform_int_distribution/operator_ltltgtgt*
