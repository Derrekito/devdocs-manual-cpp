# std::discard_block_engine<Engine,P,R>::discard

```cpp
void discard( unsigned long long z );  // (since C++11)
```

Advances the internal state by `z` times. Equivalent to calling `operator()` `z`
times and discarding the result. The state of the underlying engine may be
advanced by more than `z` times.

### Parameters

- **z** — integer value specifying the number of times to advance the state by

### Return value

(none)

### Exceptions

Throws nothing.

### See also

- **operator() (C++11)** — advances the state of the underlying engine and
  returns the generated value (public member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/discard_block_engine/discard*
