# std::expected<T,E>::or_else

```cpp
template< class F >
constexpr auto or_else( F&& f ) &;  // (1) (since C++23)
template< class F >
constexpr auto or_else( F&& f ) const&;  // (2) (since C++23)
template< class F >
constexpr auto or_else( F&& f ) &&;  // (3) (since C++23)
template< class F >
constexpr auto or_else( F&& f ) const&&;  // (4) (since C++23)
```

If `*this` contains an unexpected value, invokes `f` with the argument `error()`
and returns its result; otherwise, returns a `std::expected` object that
contains a copy of the contained expected value (obtained from `operator*`).

1,2) Given type `G` as `std::remove_cvref_t<std::invoke_result_t<F,
   decltype(error())>>`.

If `G` is not a specialization of `std::expected`, or
   `std::is_same_v<G::value_type, T>` is `false`, the program is ill-formed.

The effect is equivalent to if (has_value()) { if constexpr (std::is_void_v<T>)
   return G(); else return G(std::in_place, **this); } else return
   std::invoke(std::forward<F>(f), error());

These overloads participate in overload resolution only if `std::is_void_v<T>`
   or `std::is_constructible_v<T, decltype(**this)>` is `true`.

3,4) Given type `G` as `std::remove_cvref_t<std::invoke_result_t<F,
   decltype(std::move(error()))>>`.

If `G` is not a specialization of `std::expected`, or
   `std::is_same_v<G::value_type, T>` is `false`, the program is ill-formed.

The effect is equivalent to if (has_value()) { if constexpr (std::is_void_v<T>)
   return G(); else return G(std::in_place, std::move(**this)); } else return
   std::invoke(std::forward<F>(f), std::move(error()));

These overloads participate in overload resolution only if `std::is_void_v<T>`
   or `std::is_constructible_v<T, decltype(std::move(**this))>` is `true`.

### Parameters

- **f** — a suitable function or Callable object that returns a `std::expected`

### Return value

The result of `f`, or a `std::expected` object that contains a copy of the
expected value, as described above.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_expected` | 202211L | (C++23) | Monadic functions for
      `std::expected`

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3938 | C++23 | `or_else` was ill-formed if `T` is not (possibly
      cv-qualified) void and `E` is not copyable | made well-formed

### See also

- **transform_error** — returns the `expected` itself if it contains an expected
  value; otherwise, returns an `expected` containing the transformed unexpected
  value (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/expected/or_else*
