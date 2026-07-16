# std::negative_binomial_distribution<IntType>::reset

```cpp
void reset();  // (since C++11)
```

Resets the internal state of the distribution object. After a call to this
function, the next call to `operator()` on the distribution object will not be
dependent on previous calls to `operator()`.

### Parameters

(none)

### Return value

(none)

### Complexity

Constant.

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/negative_binomial_distribution/reset*
