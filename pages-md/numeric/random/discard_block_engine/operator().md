# std::discard_block_engine<Engine,P,R>::operator()

```cpp
result_type operator()();  // (since C++11)
```

Generates a random value. The state of the underlying engine is advanced one or
more times.

### Parameters

(none)

### Return value

A pseudo-random number in [`min()`, `max()`].

### Exceptions

Throws nothing.

### See also

- **discard (C++11)** — advances the adaptor's state by a specified amount
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/discard_block_engine/operator()*
