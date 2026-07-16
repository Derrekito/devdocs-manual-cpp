# std::discard_block_engine<Engine,P,R>::discard_block_engine

```cpp
discard_block_engine();  // (1) (since C++11)
explicit discard_block_engine( result_type s );  // (2) (since C++11)
template< class Sseq >
explicit discard_block_engine( Sseq& seq );  // (3) (since C++11)
explicit discard_block_engine( const Engine& e );  // (4) (since C++11)
explicit discard_block_engine( Engine&& e );  // (5) (since C++11)
```

Constructs new pseudo-random engine adaptor.

1) Default constructor. The underlying engine is also default-constructed.

2) Constructs the underlying engine with `s`.

3) Constructs the underlying engine with seed sequence `seq`. This constructor
   only participate in overload resolution if `Sseq` qualifies as a
   SeedSequence. In particular, this constructor does not participate in
   overload resolution if `Sseq` is implicitly convertible to `result_type`.

4) Constructs the underlying engine with a copy of `e`.

5) Move-constructs the underlying engine with `e`. `e` holds unspecified, but
   valid state afterwards.

### Parameters

- **s** — integer value to construct the underlying engine with
- **seq** — seed sequence to construct the underlying engine with
- **e** — pseudo-random number engine to initialize with

### Example

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/discard_block_engine/discard_block_engine*
