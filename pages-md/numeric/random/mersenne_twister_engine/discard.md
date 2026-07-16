# std::mersenne_twister_engine<UIntType,w,n,m,r,a,u,d,s,b,t,c,l,f>::discard

```cpp
void discard( unsigned long long z );  // (since C++11)
```

Advances the internal state by `z` times. Equivalent to calling `operator()` `z`
times and discarding the result.

### Parameters

- **z** — integer value specifying the number of times to advance the state by

### Return value

(none)

### Complexity

No worse than the complexity of `z` consecutive calls to `operator()`.

### Notes

For some engines, "fast jump" algorithms are known, which advance the state by
many steps (order of millions) without calculating intermediate state
transitions, although not necessarily in constant time.

### See also

- **operator() (C++11)** — advances the engine's state and returns the generated
  value (public member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/mersenne_twister_engine/discard*
