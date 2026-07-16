# std::function<R(Args...)>::function

```cpp
function() noexcept;  // (1) (since C++11)
function( std::nullptr_t ) noexcept;  // (2) (since C++11)
function( const function& other );  // (3) (since C++11)
function( function&& other );  // (since C++11) (until C++20)
function( function&& other ) noexcept;  // (since C++20)
template< class F >
function( F&& f );  // (5) (since C++11)
template< class Alloc >
function( std::allocator_arg_t, const Alloc& alloc ) noexcept;  // (6) (since C++11) (removed in C++17)
template< class Alloc >
function( std::allocator_arg_t, const Alloc& alloc,
          std::nullptr_t ) noexcept;  // (7) (since C++11) (removed in C++17)
template< class Alloc >
function( std::allocator_arg_t, const Alloc& alloc,
          const function& other );  // (8) (since C++11) (removed in C++17)
template< class Alloc >
function( std::allocator_arg_t, const Alloc& alloc,
          function&& other );  // (9) (since C++11) (removed in C++17)
template< class F, class Alloc >
function( std::allocator_arg_t, const Alloc& alloc, F f );  // (10) (since C++11) (removed in C++17)
```

Constructs a `std::function` from a variety of sources.

1,2) Creates an *empty* function.

3,4) Copies (3) or moves (4) the *target* of `other` to the *target* of `*this`.
   If `other` is *empty*, `*this` will be *empty* after the call too. For (4),
   `other` is in a valid but unspecified state after the call.

5) Initializes the *target* with `std::forward<F>(f)`. The *target* is of type
   `std::decay<F>::type`. If `f` is a null pointer to function, a null pointer
   to member, or an *empty* value of some `std::function` specialization,
   `*this` will be *empty* after the call. This constructor does not participate
   in overload resolution unless the target type is not same as `function`, and
   its lvalue is Callable for argument types `Args...` and return type `R`. The
   program is ill-formed if the target type is not copy-constructible or
   initialization of the *target* is ill-formed.

6-10) Same as (1-5) except that `alloc` is used to allocate memory for any
   internal data structures that the `function` might use.

When the *target* is a function pointer or a `std::reference_wrapper`, small
object optimization is guaranteed, that is, these targets are always directly
stored inside the `std::function` object, no dynamic allocation takes place.
Other large objects may be constructed in dynamic allocated storage and accessed
by the `std::function` object through a pointer.

### Parameters

- **other** — the function object used to initialize `*this`
- **f** — a callable object used to initialize `*this`
- **alloc** — an Allocator used for internal memory allocation

**Type requirements**

**-`std::decay<F>::type` must meet the requirements of Callable and CopyConstructible.**

**-`Alloc` must meet the requirements of Allocator.**

### Exceptions

3,8,9) Does not throw if `other`'s *target* is a function pointer or a
   `std::reference_wrapper`, otherwise may throw `std::bad_alloc` or any
   exception thrown by the constructor used to copy or move the stored callable
   object.

4) Does not throw if `other`'s *target* is a function pointer or a
`std::reference_wrapper`, otherwise may throw `std::bad_alloc` or any exception
thrown by the constructor used to copy or move the stored callable object.
*(until C++20)*

5,10) Does not throw if `f` is a function pointer or a `std::reference_wrapper`,
   otherwise may throw `std::bad_alloc` or any exception thrown by the copy
   constructor of the stored callable object.

### Notes

`std::function`'s allocator support was poorly specified and inconsistently
implemented. Some implementations do not provide overloads (6-10) at all, some
provide the overloads but ignore the supplied allocator argument, and some
provide the overloads and use the supplied allocator for construction but not
when the `std::function` is reassigned. As a result, allocator support was
removed in C++17.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2132 | C++11 | constructor taking a Callable object might be ambiguous |
      constrained
  LWG 2774 | C++11 | constructor taking a Callable performed an additional move
      | eliminated

### See also

- **(constructor) (C++23)** — constructs a new `std::move_only_function` object
  (public member function of `std::move_only_function`)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/function/function*
