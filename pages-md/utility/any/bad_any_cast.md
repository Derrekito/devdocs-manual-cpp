# std::bad_any_cast

```cpp
class bad_any_cast : public std::bad_cast;  // (since C++17)
```

Defines a type of object to be thrown by the value-returning forms of
`std::any_cast` on failure.

### Member functions

- **(constructor)** — constructs a new `bad_any_cast` object (public member
  function)
- **operator=** — replaces the `bad_any_cast` object (public member function)
- **what** — returns the explanatory string (public member function)

## std::bad_any_cast::bad_any_cast

```cpp
bad_any_cast() noexcept;  // (1) (since C++17)
bad_any_cast( const bad_any_cast& other ) noexcept;  // (2) (since C++17)
```

Constructs a new `bad_any_cast` object with an implementation-defined
null-terminated byte string which is accessible through `what()`.

1) Default constructor.

2) Copy constructor. If `*this` and `other` both have dynamic type
   `std::bad_any_cast` then `std::strcmp(what(), other.what()) == 0`.

### Parameters

- **other** — another exception object to copy

## std::bad_any_cast::operator=

```cpp
bad_any_cast& operator=( const bad_any_cast& other ) noexcept;  // (since C++17)
```

Assigns the contents with those of `other`. If `*this` and `other` both have
dynamic type `std::bad_any_cast` then `std::strcmp(what(), other.what()) == 0`
after assignment.

### Parameters

- **other** — another exception object to assign with

### Return value

`*this`

## std::bad_any_cast::what

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

## Inherited from std::bad_cast

## Inherited from std::exception

### Member functions

- **(destructor) [virtual]** — destroys the exception object (virtual public
  member function of `std::exception`)
- **what [virtual]** — returns an explanatory string (virtual public member
  function of `std::exception`)

---
*Source: https://en.cppreference.com/w/cpp/utility/any/bad_any_cast*
