# std::optional<T>::or_else

```cpp
template< class F >
constexpr optional or_else( F&& f ) const&;  // (1) (since C++23)
template< class F >
constexpr optional or_else( F&& f ) &&;  // (2) (since C++23)
```

Returns `*this` if it contains a value. Otherwise, returns the result of `f`.

The program is ill-formed if `std::remove_cvref_t<std::invoke_result_t<F>>` is
not same as `std::optional<T>`.

1) Equivalent to `return *this ? *this : std::forward<F>(f)();`. This overload
   participates in overload resolution only if both `std::copy_constructible<T>`
   and `std::invocable<F>` are modeled.

2) Equivalent to `return *this ? std::move(*this) : std::forward<F>(f)();`. This
   overload participates in overload resolution only if both
   `std::move_constructible<T>` and `std::invocable<F>` are modeled.

### Parameters

- **f** — a function or Callable object that returns an `std::optional<T>`

### Return value

`*this` or the result of `f`, as described above.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_optional` | 202110L | (C++23) | Monadic operations in
      `std::optional`

### Example

### See also

- **value_or** — returns the contained value if available, another value
  otherwise (public member function)
- **and_then (C++23)** — returns the result of the given function on the
  contained value if it exists, or an empty `optional` otherwise (public member
  function)
- **transform (C++23)** — returns an `optional` containing the transformed
  contained value if it exists, or an empty `optional` otherwise (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/utility/optional/or_else*
