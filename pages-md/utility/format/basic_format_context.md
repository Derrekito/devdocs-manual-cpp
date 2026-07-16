# std::basic_format_context

```cpp
template< class OutputIt, class CharT >
class basic_format_context;  // (1) (since C++20)
using format_context = basic_format_context</* unspecified */, char>;  // (2) (since C++20)
using wformat_context = basic_format_context</* unspecified */, wchar_t>;  // (3) (since C++20)
```

Provides access to formatting state consisting of the formatting arguments and
the output iterator.

The behavior is undefined if `OutputIt` does not model
`std::output_iterator<const CharT&>`.

2) The unspecified template argument is an output iterator that appends to
   `std::string`, such as `std::back_insert_iterator<std::string>`.
   Implementations typically use an iterator to type-erased buffer type that
   supports appending to any contiguous and resizable container.

3) The unspecified template argument is an output iterator that appends to
   `std::wstring`.

### Member types

- **`iterator`** — `OutputIt`
- **`char_type`** — `CharT`

### Member alias templates

- **formatter_type<T>** — std::formatter<T, CharT>

### Member functions

- **arg** — returns the argument at the given index (public member function)
- **locale** — returns the locale used for locale-specific formatting (public
  member function)
- **out** — returns the iterator to output buffer (public member function)
- **advance_to** — advances the output iterator to the given position (public
  member function)

## std::basic_format_context::arg

```cpp
std::basic_format_arg<basic_format_context> arg( std::size_t id ) const;
```

Returns a `std::basic_format_arg` holding the `id`-th argument in `args`, where
`args` is the parameter pack or `std::basic_format_args` object passed to the
formatting function.

If `id` is not less than the number of formatting arguments, returns a
default-constructed `std::basic_format_arg` (holding a `std::monostate` object).

## std::basic_format_context::locale

```cpp
std::locale locale();
```

Returns the locale passed to the formatting function, or a default-constructed
`std::locale` if the formatting function does not take a locale.

## std::basic_format_context::out

```cpp
iterator out();
```

Returns the iterator to the output buffer. The result is move-constructed from
the stored iterator.

## std::basic_format_context::advance_to

```cpp
void advance_to( iterator it );
```

Move assigns `it` to the stored output iterator. After a call to `advance_to`,
the next call to `out()` will return an iterator with the value that `it` had
before the assignment.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3567 | C++20 | `basic_format_context` does not work move-only iterator
      types | made to move iterators

---
*Source: https://en.cppreference.com/w/cpp/utility/format/basic_format_context*
