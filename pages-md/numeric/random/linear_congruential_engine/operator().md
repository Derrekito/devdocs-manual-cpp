# std::linear_congruential_engine<UIntType,a,c,m>::operator()

```cpp
result_type operator()();  // (since C++11)
```

Generates a pseudo-random value. The state of the engine is advanced by one
position.

### Parameters

(none)

### Return value

A pseudo-random number in [`min()`, `max()`].

### Complexity

Amortized constant.

### See also

- **discard (C++11)** — advances the engine's state by a specified amount
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/linear_congruential_engine/operator()*
