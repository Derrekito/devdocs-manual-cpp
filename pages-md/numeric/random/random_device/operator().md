# std::random_device::operator()

```cpp
result_type operator()();  // (since C++11)
```

Generates a non-deterministic uniformly-distributed random value.

### Parameters

(none)

### Return value

A random number uniformly distributed in [`min()`, `max()`].

### Exceptions

Throws an implementation-defined exception derived from `std::exception` if a
random number could not be generated.

### See also

- **min [static] (C++11)** — gets the smallest possible value in the output
  range (public static member function)
- **max [static] (C++11)** — gets the largest possible value in the output range
  (public static member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/random_device/operator()*
