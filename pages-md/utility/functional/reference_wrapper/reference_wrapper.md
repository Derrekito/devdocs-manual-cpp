# std::reference_wrapper<T>::reference_wrapper

```cpp
template< class U >
reference_wrapper( U&& x ) noexcept(/*see below*/) ;  // (since C++11) (until C++20)
template< class U >
constexpr reference_wrapper( U&& x ) noexcept(/*see below*/) ;  // (since C++20)
reference_wrapper( const reference_wrapper& other ) noexcept;  // (since C++11) (until C++20)
constexpr reference_wrapper( const reference_wrapper& other ) noexcept;  // (since C++20)
```

Constructs a new reference wrapper.

1) Converts `x` to `T&` as if by `T& t = std::forward<U>(x);`, then stores a
   reference to `t`. This overload participates in overload resolution only if
   `typename std::decay<U>::type` is not the same type as `reference_wrapper`
   and the expression `FUN(std::declval<U>())` is well-formed, where `FUN` names
   the set of imaginary functions void FUN(T&) noexcept; void FUN(T&&) = delete;

2) Copy constructor. Stores a reference to `other.get()`.

### Parameters

- **x** — an object to wrap
- **other** — another reference wrapper

### Exceptions

1) `noexcept` specification: `noexcept(noexcept(FUN(std::declval<U>())))` where
   `FUN` is the set of imaginary functions described in the description above.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2993 | C++11 | deleted `reference_wrapper(T&&)` constructor interferes
      with overload resolution in some cases | replaced with a constructor
      template

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/reference_wrapper/reference_wrapper*
