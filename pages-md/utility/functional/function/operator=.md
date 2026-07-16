# std::function<R(Args...)>::operator=

```cpp
function& operator=( const function& other );  // (1) (since C++11)
function& operator=( function&& other );  // (2) (since C++11)
function& operator=( std::nullptr_t ) noexcept;  // (3) (since C++11)
template< class F >
function& operator=( F&& f );  // (4) (since C++11)
template< class F >
function& operator=( std::reference_wrapper<F> f ) noexcept;  // (5) (since C++11)
```

Assigns a new *target* to `std::function`.

1) Assigns a copy of *target* of `other`, as if by executing
   `function(other).swap(*this);`

2) Moves the *target* of `other` to `*this`. `other` is in a valid state with an
   unspecified value.

3) Drops the current *target*. `*this` is *empty* after the call.

4) Sets the *target* of `*this` to the callable `f`, as if by executing
   `function(std::forward<F>(f)).swap(*this);`. This operator does not
   participate in overload resolution unless `f` is Callable for argument types
   `Args...` and return type `R`.

5) Sets the *target* of `*this` to a copy of `f`, as if by executing
   `function(f).swap(*this);`

### Parameters

- **other** — another `std::function` object to copy the target of
- **f** — a callable to initialize the *target* with

**Type requirements**

**-`F` must meet the requirements of Callable.**

### Return value

`*this`

### Notes

Even before allocator support was removed from `std::function` in C++17, these
assignment operators use the default allocator rather than the allocator of
`*this` or the allocator of `other` (see LWG issue 2386).

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2132 | C++11 | the overload taking a Callable object might be ambiguous |
      constrained
  LWG 2401 | C++11 | assignment operator from `std::nullptr_t` not required to
      be noexcept | required

### See also

- **operator= (C++23)** — replaces or destroys the target (public member
  function of `std::move_only_function`)
- **assign (removed in C++17)** — assigns a new target (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/function/operator%3D*
