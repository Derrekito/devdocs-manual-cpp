# std::expected<T,E>::transform_error

```cpp
template< class F >
constexpr auto transform_error( F&& f ) &;  // (1) (since C++23)
template< class F >
constexpr auto transform_error( F&& f ) const&;  // (2) (since C++23)
template< class F >
constexpr auto transform_error( F&& f ) &&;  // (3) (since C++23)
template< class F >
constexpr auto transform_error( F&& f ) const&&;  // (4) (since C++23)
```

If `*this` contains an error value, invokes `f` with the argument `error()` and
returns a `std::expected` object that contains its result; otherwise, returns a
`std::expected` object that contains a copy of the contained expected value
(obtained from `operator*`).

1,2) Given type `G` as `std::remove_cv_t<std::invoke_result_t<F,
   decltype(error())>>`.

If `G` is not a valid template argument for `std::unexpected`, or `G
   g(std::invoke(std::forward<F>(f), error()));` is ill-formed, the program is
   ill-formed.

The effect is equivalent to if (has_value()) { if (std::is_void_v<T>) return
   std::expected<T, G>(); else return std::expected<T, G>(std::in_place,
   **this); } else // the returned std::expected object contains an unexpected
   value, // which is direct-non-list-initialized with //
   std::invoke(std::forward<F>(f), error()) return /* an std::expected<T, G>
   object */;

These overloads participate in overload resolution only if `std::is_void_v<T>`
   or `std::is_constructible_v<T, decltype(**this)>` is `true`.

3,4) Given type `G` as `std::remove_cv_t<std::invoke_result_t<F,
   decltype(std::move(error()))>>`.

If `G` is not a valid template argument for `std::unexpected`, or `G
   g(std::invoke(std::forward<F>(f), std::move(error())));` is ill-formed, the
   program is ill-formed.

The effect is equivalent to if (has_value()) { if (std::is_void_v<T>) return
   std::expected<T, G>(); else return std::expected<T, G>(std::in_place,
   std::move(**this)); } else // the returned std::expected object contains an
   unexpected value, // which is direct-non-list-initialized with //
   std::invoke(std::forward<F>(f), std::move(error())) return /* an
   std::expected<T, G> object */; These overloads participate in overload
   resolution only if `std::is_void_v<T>` or `std::is_constructible_v<T,
   decltype(std::move(**this))>` is `true`.

### Parameters

- **f** — a suitable function or Callable object whose call signature returns a
  non-reference type

### Return value

A `std::expected` object containing either the result of `f` or an expected
value, as described above.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3938 | C++23 | `transform_error` was ill-formed if `T` is not (possibly
      cv-qualified) void and `E` is not copyable | made well-formed

### See also

- **or_else** — returns the `expected` itself if it contains an expected value;
  otherwise, returns the result of the given function on the unexpected value
  (public member function)
- **transform** — returns an `expected` containing the transformed expected
  value if it exists; otherwise, returns the `expected` itself (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/utility/expected/transform_error*
