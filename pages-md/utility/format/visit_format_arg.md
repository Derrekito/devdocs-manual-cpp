# std::visit_format_arg

```cpp
template< class Visitor, class Context >
/* see below */ visit_format_arg( Visitor&& vis, std::basic_format_arg<Context> arg );  // (since C++20) (deprecated in C++26)
```

Applies the visitor `vis` to the object contained in `arg`.

Equivalent to `std::visit(std::forward<Visitor>(vis), value)`, where `value` is
the `std::variant` stored in `arg`.

### Parameters

- **vis** — a Callable that accepts every possible alternative from `arg`
- **arg** — a `std::basic_format_arg` to be visited

### Return value

The value returned by the selected invocation of the visitor.

### Notes

As of C++26, `std::visit_format_arg` is deprecated in favor of the `visit`
member functions of `std::basic_format_arg`.

### Example

### See also

- **make_format_argsmake_wformat_args (C++20)(C++20)** — creates a type-erased
  object referencing all formatting arguments, convertible to `format_args`
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/format/visit_format_arg*
