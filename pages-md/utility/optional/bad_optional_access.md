# std::bad_optional_access

```cpp
class bad_optional_access;  // (since C++17)
```

Defines a type of object to be thrown by `std::optional::value` when accessing
an optional object that does not contain a value.

### Member functions

- **(constructor)** — constructs a new `bad_optional_access` object (public
  member function)
- **operator=** — replaces the `bad_optional_access` object (public member
  function)
- **what** — returns the explanatory string (public member function)

## std::bad_optional_access::bad_optional_access

```cpp
bad_optional_access() noexcept;  // (1) (since C++17)
bad_optional_access( const bad_optional_access& other ) noexcept;  // (2) (since C++17)
```

Constructs a new `bad_optional_access` object with an implementation-defined
null-terminated byte string which is accessible through `what()`.

1) Default constructor.

2) Copy constructor. If `*this` and `other` both have dynamic type
   `std::bad_optional_access` then `std::strcmp(what(), other.what()) == 0`.

### Parameters

- **other** — another exception object to copy

## std::bad_optional_access::operator=

```cpp
bad_optional_access& operator=( const bad_optional_access& other ) noexcept;  // (since C++17)
```

Assigns the contents with those of `other`. If `*this` and `other` both have
dynamic type `std::bad_optional_access` then `std::strcmp(what(), other.what())
== 0` after assignment.

### Parameters

- **other** — another exception object to assign with

### Return value

`*this`

## std::bad_optional_access::what

```cpp
virtual const char* what() const noexcept;  // (since C++17)
```

Returns the explanatory string.

### Parameters

(none)

### Return value

Pointer to a null-terminated string with explanatory information. The string is
suitable for conversion and display as a `std::wstring`. The pointer is
guaranteed to be valid at least until the exception object from which it is
obtained is destroyed, or until a non-const member function (e.g. copy
assignment operator) on the exception object is called.

### Notes

Implementations are allowed but not required to override `what()`.

## Inherited from std::exception

### Member functions

- **(destructor) [virtual]** — destroys the exception object (virtual public
  member function of `std::exception`)
- **what [virtual]** — returns an explanatory string (virtual public member
  function of `std::exception`)

---
*Source: https://en.cppreference.com/w/cpp/utility/optional/bad_optional_access*
