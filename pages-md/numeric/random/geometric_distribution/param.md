# std::geometric_distribution<IntType>::param

```cpp
param_type param() const;  // (1) (since C++11)
void param( const param_type& params );  // (2) (since C++11)
```

Manages the associated distribution parameter set.

1) Returns the associated parameter set.

2) Sets the associated parameter set to `params`.

### Parameters

- **params** — new contents of the associated parameter set

### Return value

1) The associated parameter set.

2) (none)

### Complexity

Constant.

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/geometric_distribution/param*
