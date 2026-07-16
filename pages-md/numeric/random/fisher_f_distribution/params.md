# std::fisher_f_distribution<RealType>::m, n

```cpp
RealType m() const;  // (1) (since C++11)
RealType n() const;  // (2) (since C++11)
```

Returns the parameters the distribution was constructed with.

1) Returns the m (the first degree of freedom) distribution parameter. The
   default value is `1.0`.

2) Returns the n (the second degree of freedom) distribution parameter. The
   default value is `1.0`.

### Parameters

(none)

### Return value

1) The m (the first degree of freedom) distribution parameter.

2) The n (the second degree of freedom) distribution parameter.

### Complexity

Constant.

### See also

- **param (C++11)** — gets or sets the distribution parameter object (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/fisher_f_distribution/params*
