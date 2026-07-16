# std::cauchy_distribution<RealType>::a, b

```cpp
RealType a() const;  // (1) (since C++11)
RealType b() const;  // (2) (since C++11)
```

Returns the distribution parameters with which the distribution was constructed.

1) Returns the *a* parameter. It specifies the location of the peak and is also
   called location parameter. The default value is `0.0`.

2) Returns the *b* parameter. It represents the half-width at half-maximum
   (HWHM) and is also called scale parameter. The default value is `1.0`.

### Parameters

(none)

### Return value

1) The value of the *a* parameter.

2) The value of the *b* parameter.

### See also

- **param (C++11)** — gets or sets the distribution parameter object (public
  member function)

### External links

  1. | Scale parameter. From Wikipedia.
  2. | Location parameter. From Wikipedia.

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/cauchy_distribution/params*
