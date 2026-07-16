# std::uniform_int_distribution<IntType>::a, b

```cpp
result_type a() const;  // (1) (since C++11)
result_type b() const;  // (2) (since C++11)
```

Returns the parameters the distribution has been constructed with.

1) Returns the *a* distribution parameter. It defines the minimum possibly
   generated value. The default value is `​0​`.

2) Returns the *b* distribution parameter. It defines the maximum possibly
   generated value. The default value is `std::numeric_limits<IntType>::max()`.

### Parameters

(none)

### Return value

1) The *a* distribution parameter.

2) The *b* distribution parameter.

### See also

- **param (C++11)** — gets or sets the distribution parameter object (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/uniform_int_distribution/params*
