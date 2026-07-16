# std::copyable_function

```cpp
template< class... >
class copyable_function; // not defined  // (1) (since C++26)
template< class R, class... Args >
class copyable_function<R(Args...)>;
template< class R, class... Args >
class copyable_function<R(Args...) noexcept>;
template< class R, class... Args >
class copyable_function<R(Args...) &>;
template< class R, class... Args >
class copyable_function<R(Args...) & noexcept>;
template< class R, class... Args >
class copyable_function<R(Args...) &&>;
template< class R, class... Args >
class copyable_function<R(Args...) && noexcept>;
template< class R, class... Args >
class copyable_function<R(Args...) const>;
template< class R, class... Args >
class copyable_function<R(Args...) const noexcept>;
template< class R, class... Args >
class copyable_function<R(Args...) const &>;
template< class R, class... Args >
class copyable_function<R(Args...) const & noexcept>;
template< class R, class... Args >
class copyable_function<R(Args...) const &&>;
template< class R, class... Args >
class copyable_function<R(Args...) const && noexcept>;  // (2) (since C++26)
```

Class template `std::copyable_function` is a general-purpose polymorphic
function wrapper. `std::copyable_function` objects can store and invoke any
CopyConstructible Callable *target* — functions, lambda expressions, bind
expressions, or other function objects, as well as pointers to member functions
and pointers to member objects.

The stored callable object is called the *target* of `std::copyable_function`.
If a `std::copyable_function` contains no target, it is called *empty*. Unlike
`std::function`, invoking an *empty* `std::copyable_function` results in
undefined behavior.

`std::copyable_function`s supports every possible combination of cv-qualifiers,
ref-qualifiers, and noexcept-specifiers not including `volatile` provided in its
template parameter. These qualifiers and specifier (if any) are added to its
`operator()`.

`std::copyable_function` satisfies the requirements of CopyConstructible and
CopyAssignable.

### Member types

- **`result_type`** — `R`

### Member functions

- **(constructor) (C++26)** — constructs a new `std::copyable_function` object
  (public member function)
- **(destructor) (C++26)** — destroys a `std::copyable_function` object (public
  member function)
- **operator= (C++26)** — replaces or destroys the target (public member
  function)
- **swap (C++26)** — swaps the targets of two `std::copyable_function` objects
  (public member function)
- **operator bool (C++26)** — checks if the `std::copyable_function` has a
  target (public member function)
- **operator() (C++26)** — invokes the target (public member function)

### Non-member functions

- **swap(std::copyable_function) (C++26)** — overloads the `std::swap` algorithm
  (function)
- **operator== (C++26)** — compares a `std::copyable_function` with `nullptr`
  (function)

### Notes

Implementations may store a callable object of small size within the
`std::copyable_function` object. Such small object optimization is effectively
required for function pointers and `std::reference_wrapper` specializations, and
can only be applied to types `T` for which
`std::is_nothrow_move_constructible_v<T>` is `true`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_copyable_function` | 202306L | (C++26) | `std::copyable_function`

### Example

### See also

- **function (C++11)** — wraps callable object of any copy constructible type
  with specified function call signature (class template)
- **move_only_function (C++23)** — wraps callable object of any type with
  specified function call signature (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/copyable_function*
