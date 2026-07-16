# std::subtract_with_carry_engine<UIntType,w,s,r>::seed

```cpp
void seed( result_type value = default_seed );  // (1) (since C++11)
template< class Sseq >
void seed( Sseq& seq );  // (2) (since C++11)
```

Reinitializes the internal state of the random-number engine using new seed
value.

### Parameters

- **value** — seed value to use in the initialization of the internal state
- **seq** — seed sequence to use in the initialization of the internal state

### Exceptions

Throws nothing.

### Complexity

Linear in the size of the state.

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/subtract_with_carry_engine/seed*
