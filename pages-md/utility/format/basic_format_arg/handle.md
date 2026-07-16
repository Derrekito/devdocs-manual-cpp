# std::basic_format_arg<Context>::handle

```cpp
template< class Context >
class basic_format_arg<Context>::handle;  // (since C++20)
```

A type-erased wrapper that allows formatting an object of a user-defined type.

`handle` objects are typically created by `std::make_format_args` and accessed
through `std::visit_format_arg` or the `visit` member functions of
`std::basic_format_arg`(since C++26).

### Data members

A typical implementation of `handle` is TriviallyCopyable and only stores two
non-static data members:

- a `const void*` pointer to the object to be formatted, and
- a `void (*)(std::basic_format_parse_context<Context::char_type>&, Context&,
  const void*)` function pointer to the function performing needed operations in
  the `format` member function (see below).

### Member functions

- **format (C++20)** — formats the referenced object with the given contexts
  (public member function)

## std::basic_format_arg<Context>::handle::format

```cpp
void format( std::basic_format_parse_context<Context::char_type>& parse_ctx,
             Context& format_ctx ) const;  // (since C++20)
```

Let

- `T` be the type of the formatting argument,
- `TD` be `std::remove_const_t<T>`,
- `TQ` be `const TD` if `const TD` satisfies `__formattable_with`<Context> or
  `TD` otherwise, and
- `ref` be a reference to the formatting argument.

Equivalent to: `typename Context::template formatter_type<TD> f;
parse_ctx.advance_to(f.parse(parse_ctx));
format_ctx.advance_to(f.format(const_cast<TQ&>(static_cast<const TD&>(ref)),
format_ctx));`

### Notes

A `handle` has reference semantics for the formatted argument and does not
extend its lifetime. It is the programmer's responsibility to ensure that the
argument outlives the `handle`. Usually, a `handle` is only used within
formatting functions.

### See also

- **basic_format_arg (C++20)** — class template that provides access to a
  formatting argument for user-defined formatters (class template)
- **make_format_argsmake_wformat_args (C++20)(C++20)** — creates a type-erased
  object referencing all formatting arguments, convertible to `format_args`
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/format/basic_format_arg/handle*
