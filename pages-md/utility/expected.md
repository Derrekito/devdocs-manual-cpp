# std::expected

```cpp
template< class T, class E >
class expected;  // (since C++23)
```

The class template `std::expected` provides a way to store either of two values.
An object of `std::expected` at any given time either holds an *expected* value
of type `T`, or an *unexpected* value of type `E`. `std::expected` is never
valueless.

The stored value is allocated directly within the storage occupied by the
`expected` object. No dynamic memory allocation takes place.

A program is ill-formed if it instantiates an `expected` with a reference type,
a function type, or a specialization of `std::unexpected`. In addition, `T` must
not be `std::in_place_t` or `std::unexpect_t`.

### Template parameters

- **T** — the type of the expected value. The type must either be (possibly
  cv-qualified) `void`, or meet the Destructible requirements (in particular,
  array and reference types are not allowed).
- **E** — the type of the unexpected value. The type must meet the Destructible
  requirements, and must be a valid template argument for `std::unexpected` (in
  particular, arrays, non-object types, and cv-qualified types are not allowed).

### Member types

- **`value_type`** — `T`
- **`error_type`** — `E`
- **`unexpected_type`** — `std::unexpected<E>`

### Member alias templates

- **rebind<U>** — expected<U, error_type>

### Member functions

- **(constructor)** — constructs the `expected` object (public member function)
- **(destructor)** — destroys the `expected` object, along with its contained
  value (public member function)
- **operator=** — assigns contents (public member function)

**Observers**

- **operator->operator*** — accesses the expected value (public member function)
- **operator boolhas_value** — checks whether the object contains an expected
  value (public member function)
- **value** — returns the expected value (public member function)
- **error** — returns the unexpected value (public member function)
- **value_or** — returns the expected value if present, another value otherwise
  (public member function)

**Monadic operations**

- **and_then** — returns the result of the given function on the expected value
  if it exists; otherwise, returns the `expected` itself (public member
  function)
- **transform** — returns an `expected` containing the transformed expected
  value if it exists; otherwise, returns the `expected` itself (public member
  function)
- **or_else** — returns the `expected` itself if it contains an expected value;
  otherwise, returns the result of the given function on the unexpected value
  (public member function)
- **transform_error** — returns the `expected` itself if it contains an expected
  value; otherwise, returns an `expected` containing the transformed unexpected
  value (public member function)

**Modifiers**

- **emplace** — constructs the expected value in-place (public member function)
- **swap** — exchanges the contents (public member function)

### Non-member functions

- **operator== (C++23)** — compares `expected` objects (function template)
- **swap(std::expected) (C++23)** — specializes the `std::swap` algorithm
  (function)

### Helper classes

- **unexpected (C++23)** — represented as an unexpected value (class template)
- **bad_expected_access (C++23)** — exception indicating checked access to an
  `expected` that contains an unexpected value (class template)
- **unexpectunexpect_t (C++23)** — in-place construction tag for unexpected
  value in `expected` (tag)

### Notes

Types with the same functionality are called `Result` in Rust and `Either` in
Haskell.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_expected` | 202202L | (C++23) | class template `std::expected` and
      associated helper classes
  202211L | (C++23) | Monadic functions for `std::expected`

### Example

```cpp
#include <cmath>
#include <expected>
#include <iomanip>
#include <iostream>
#include <string_view>

enum class parse_error
{
    invalid_input,
    overflow
};

auto parse_number(std::string_view& str) -> std::expected<double, parse_error>
{
    const char* begin = str.data();
    char* end;
    double retval = std::strtod(begin, &end);

    if (begin == end)
        return std::unexpected(parse_error::invalid_input);
    else if (std::isinf(retval))
        return std::unexpected(parse_error::overflow);

    str.remove_prefix(end - begin);
    return retval;
}

int main()
{
    auto process = [](std::string_view str)
    {
        std::cout << "str: " << std::quoted(str) << ", ";
        if (const auto num = parse_number(str); num.has_value())
            std::cout << "value: " << *num << '\n';
            // If num did not have a value, dereferencing num
            // would cause an undefined behavior, and
            // num.value() would throw std::bad_expected_access.
            // num.value_or(123) uses specified default value 123.
        else if (num.error() == parse_error::invalid_input)
            std::cout << "error: invalid input\n";
        else if (num.error() == parse_error::overflow)
            std::cout << "error: overflow\n";
        else
            std::cout << "unexpected!\n"; // or invoke std::unreachable();
    };

    for (auto src : {"42", "42abc", "meow", "inf"})
        process(src);
}
```

Output:

```text
str: "42", value: 42
str: "42abc", value: 42
str: "meow", error: invalid input
str: "inf", error: overflow
```

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

- **variant (C++17)** — a type-safe discriminated union (class template)
- **optional (C++17)** — a wrapper that may or may not hold an object (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/utility/expected*
