# operator<<,>>(std::mersenne_twister_engine)

```cpp
template< class CharT, class Traits >
friend std::basic_ostream<CharT,Traits>&
    operator<<( std::basic_ostream<CharT,Traits>& ost,
                const mersenne_twister_engine& e );  // (1) (since C++11)
template< class CharT, class Traits >
friend std::basic_istream<CharT,Traits>&
    operator>>( std::basic_istream<CharT,Traits>& ist,
                mersenne_twister_engine& e );  // (2) (since C++11)
```

1) Serializes the internal state of the pseudo-random number engine `e` as a
   sequence of decimal numbers separated by one or more spaces, and inserts it
   to the stream `ost`. The fill character and the formatting flags of the
   stream are ignored and unaffected.

2) Restores the internal state of the pseudo-random number engine `e` from the
   serialized representation, which was created by an earlier call to
   `operator<<` using a stream with the same imbued locale and the same `CharT`
   and `Traits`. If the input cannot be deserialized, `e` is left unchanged and
   `failbit` is raised on `ist`.

These function templates are not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`std::mersenne_twister_engine<UIntType,w,n,m,r,a,u,d,s,b,t,c,l,f>` is an
associated class of the arguments.

If a textual representation is written using `os << x` and that representation
is restored into the same or a different object `y` of the same type using `is
>> y`, then `x == y`.

The textual representation is written with `os.fmtflags` set to
`ios_base::dec`|`ios_base::left` and the fill character set to the space
character. The textual representation of the engine's internal state is a set of
decimal numbers separated by spaces.

### Parameters

- **ost** — output stream to insert the data to
- **ist** — input stream to extract the data from
- **e** — pseudo-random number engine

### Return value

1) `ost`

2) `ist`

### Complexity

### Exceptions

1) May throw implementation-defined exceptions.

2) May throw `std::ios_base::failure` when setting `failbit`.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3519 | C++11 | the form of insertion and extraction operators were
      unspecified | specified to be hidden friends

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/mersenne_twister_engine/operator_ltltgtgt*
