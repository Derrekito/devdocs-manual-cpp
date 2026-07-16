# std::chrono::time_point<Clock,Duration>::operator+=, operator-=

```cpp
time_point& operator+=( const duration& d );  // (1) (since C++11) (constexpr since C++17)
time_point& operator-=( const duration& d );  // (2) (since C++11) (constexpr since C++17)
```

Modifies the time point by the given duration.

1) Applies the offset `d` to `pt`. Effectively, `d` is added to the internally
   stored duration `d_` as `d_ += d`.

2) Applies the offset `d` to `pt` in negative direction. Effectively, `d` is
   subtracted from internally stored duration `d_` as `d_ -= d`.

### Parameters

- **d** — a time offset to apply

### Return value

`*this`

### Exceptions

May throw implementation-defined exceptions.

### Example

### See also

- **operator+operator- (C++11)** — performs add and subtract operations
  involving a time point (function template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/time_point/operator_arith*
