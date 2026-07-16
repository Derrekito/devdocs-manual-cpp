# std::optional

```cpp
template< class T >
class optional;  // (since C++17)
```

The class template `std::optional` manages an *optional* contained value, i.e. a
value that may or may not be present.

A common use case for `optional` is the return value of a function that may
fail. As opposed to other approaches, such as std::pair<T, bool>, `optional`
handles expensive-to-construct objects well and is more readable, as the intent
is expressed explicitly.

Any instance of `optional<T>` at any given point in time either *contains a
value* or *does not contain a value*.

If an `optional<T>` *contains a value*, the value is guaranteed to be allocated
as part of the `optional` object footprint, i.e. no dynamic memory allocation
ever takes place. Thus, an `optional` object models an object, not a pointer,
even though `operator*()` and `operator->()` are defined.

When an object of type `optional<T>` is contextually converted to bool, the
conversion returns `true` if the object *contains a value* and `false` if it
*does not contain a value*.

The `optional` object *contains a value* in the following conditions:

- The object is initialized with/assigned from a value of type `T` or another
  `optional` that *contains a value*.

The object *does not contain a value* in the following conditions:

- The object is default-initialized.
- The object is initialized with/assigned from a value of type `std::nullopt_t`
  or an `optional` object that *does not contain a value*.
- The member function `reset()` is called.

There are no optional references; a program is ill-formed if it instantiates an
`optional` with a reference type. In addition, a program is ill-formed if it
instantiates an `optional` with the (possibly cv-qualified) tag types
`std::nullopt_t` or `std::in_place_t`.

### Template parameters

- **T** — the type of the value to manage initialization state for. The type
  must meet the requirements of Destructible (in particular, array and reference
  types are not allowed).

### Member types

- **`value_type`** — `T`

### Member functions

- **(constructor)** — constructs the optional object (public member function)
- **(destructor)** — destroys the contained value, if there is one (public
  member function)
- **operator=** — assigns contents (public member function)

**Observers**

- **operator->operator*** — accesses the contained value (public member
  function)
- **operator boolhas_value** — checks whether the object contains a value
  (public member function)
- **value** — returns the contained value (public member function)
- **value_or** — returns the contained value if available, another value
  otherwise (public member function)

**Monadic operations**

- **and_then (C++23)** — returns the result of the given function on the
  contained value if it exists, or an empty `optional` otherwise (public member
  function)
- **transform (C++23)** — returns an `optional` containing the transformed
  contained value if it exists, or an empty `optional` otherwise (public member
  function)
- **or_else (C++23)** — returns the `optional` itself if it contains a value, or
  the result of the given function otherwise (public member function)

**Modifiers**

- **swap** — exchanges the contents (public member function)
- **reset** — destroys any contained value (public member function)
- **emplace** — constructs the contained value in-place (public member function)

### Non-member functions

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++17)(C++17)(C++17)(C++17)(C++17)(C++17)(C++20)** — compares `optional`
  objects (function template)
- **make_optional (C++17)** — creates an `optional` object (function template)
- **std::swap(std::optional) (C++17)** — specializes the `std::swap` algorithm
  (function template)

### Helper classes

- **std::hash<std::optional> (C++17)** — hash support for **`std::optional`**
  (class template specialization)
- **nullopt_t (C++17)** — indicator of optional type with uninitialized state
  (class)
- **bad_optional_access (C++17)** — exception indicating checked access to an
  optional that doesn't contain a value (class)

### Helpers

- **nullopt (C++17)** — an object of type `nullopt_t` (constant)
- **in_placein_place_typein_place_indexin_place_tin_place_type_tin_place_index_t
  (C++17)** — in-place construction tag (tag)

### Deduction guides

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_optional` | 201606L | (C++17) | `std::optional`
  `__cpp_lib_optional` | 202106L | (C++20) (DR) | Fully constexpr
  `__cpp_lib_optional` | 202110L | (C++23) | Monadic operations

### Example

```cpp
#include <iostream>
#include <optional>
#include <string>

// optional can be used as the return type of a factory that may fail
std::optional<std::string> create(bool b)
{
    if (b)
        return "Godzilla";
    return {};
}

// std::nullopt can be used to create any (empty) std::optional
auto create2(bool b)
{
    return b ? std::optional<std::string>{"Godzilla"} : std::nullopt;
}

int main()
{
    std::cout << "create(false) returned "
              << create(false).value_or("empty") << '\n';

    // optional-returning factory functions are usable as conditions of while and if
    if (auto str = create2(true))
        std::cout << "create2(true) returned " << *str << '\n';
}
```

Output:

```text
create(false) returned empty
create2(true) returned Godzilla
```

### See also

- **variant (C++17)** — a type-safe discriminated union (class template)
- **any (C++17)** — objects that hold instances of any CopyConstructible type
  (class)

---
*Source: https://en.cppreference.com/w/cpp/utility/optional*
