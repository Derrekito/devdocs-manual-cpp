# std::basic_format_arg

```cpp
template< class Context >
class basic_format_arg;  // (since C++20)
```

Provides access to a formatting argument.

`basic_format_arg` objects are typically created by `std::make_format_args` and
accessed through `std::visit_format_arg` or the `visit` member functions(since
C++26).

A `basic_format_arg` object behaves as if it stores a `std::variant` of the
following types:

- `std::monostate` (only if the object was default-constructed)
- `bool`
- `Context::char_type`
- `int`
- `unsigned int`
- `long long int`
- `unsigned long long int`
- `float`
- `double`
- `long double`
- `const Context::char_type*`
- `std::basic_string_view<Context::char_type>`
- `const void*`
- `basic_format_arg::handle`

### Member classes

- **handle (C++20)** — type-erased wrapper that allows formatting an object of
  user-defined type (public member class)

### Member functions

- **(constructor) (C++20)** — constructs a `std::basic_format_arg` (public
  member function)
- **operator bool (C++20)** — checks if the current object holds a formatting
  argument (public member function)
- **visit (C++26)** — visit the stored formatting argument (public member
  function)

### Non-member functions

- **visit_format_arg (C++20) (deprecated in C++26)** — argument visitation
  interface for user-defined formatters (function template)

## std::basic_format_arg::basic_format_arg

```cpp
basic_format_arg() noexcept;  // (since C++20)
```

Default constructor. Constructs a `basic_format_arg` that does not hold a
formatting argument. The stored object has type `std::monostate`.

To create a `basic_format_arg` that holds a formatting argument,
`std::make_format_args` has to be used.

## std::basic_format_arg::operator bool

```cpp
explicit operator bool() const noexcept;  // (since C++20)
```

Checks whether `*this` holds a formatting argument.

Returns `true` if `*this` holds a formatting argument (i.e. the stored object
does not have type `std::monostate`), `false` otherwise.

## std::basic_format_arg::visit

```cpp
template< class Visitor >
decltype(auto) visit( this basic_format_arg arg, Visitor&& vis );  // (1) (since C++26)
template< class R, class Visitor >
R visit( this basic_format_arg arg, Visitor&& vis );  // (2) (since C++26)
```

Applies the visitor `vis` to the object contained in `arg`.

The `visit` functions do not modify the `basic_format_arg` object on which it is
called because a copy of the object is used when calling `vis`.

1) Equivalent to `return std::visit(std::forward<Visitor>(vis), v);`, where `v`
   is the `std::variant` stored in `arg`.

2) Equivalent to `return std::visit<R>(std::forward<Visitor>(vis), v);`, where
   `v` is the `std::variant` stored in `arg`.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_format` | 202306L | (C++26) | Member `visit`

### Example

### See also

- **basic_format_argsformat_argswformat_args (C++20)(C++20)(C++20)** — class
  that provides access to all formatting arguments (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/format/basic_format_arg*
