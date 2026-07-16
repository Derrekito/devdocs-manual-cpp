# std::independent_bits_engine<Engine,W,UIntType>::seed

```cpp
void seed();  // (1) (since C++11)
void seed( result_type value );  // (2) (since C++11)
template< class Sseq >
void seed( Sseq& seq );  // (3) (since C++11)
```

Reinitializes the internal state of the underlying engine using a new seed
value.

1) Seeds the underlying engine with the default seed value. Effectively calls
   `e.seed()`, where `e` is the underlying engine.

2) Seeds the underlying engine with the seed value `s`. Effectively calls
   `e.seed(value)`, where `e` is the underlying engine.

3) Seeds the underlying engine with the seed sequence `seq`. Effectively calls
   `e.seed(seq)`, where `e` is the underlying engine. This template only
   participate in overload resolution if `Sseq` qualifies as a SeedSequence. In
   particular, this template does not participate in overload resolution if
   `Sseq` is implicitly convertible to `result_type`.

### Parameters

- **value** — seed value to use in the initialization of the internal state of
  the underlying engine
- **seq** — seed sequence to use in the initialization of the internal state of
  the underlying engine

### Return value

(none)

### Exceptions

Throws nothing.

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/independent_bits_engine/seed*
