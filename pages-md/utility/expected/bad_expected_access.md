# std::bad_expected_access

```cpp
template< class E >
class bad_expected_access : public std::bad_expected_access<void>  // (1) (since C++23)
template<>
class bad_expected_access<void> : public std::exception  // (2) (since C++23)
```

1) Defines a type of object to be thrown by `std::expected::value` when
   accessing an expected object that contains an unexpected value.
   `bad_expected_access<E>` stores a copy of the unexpected value.

2) `bad_expected_access<void>` is the base class of all other
   `bad_expected_access` specializations.

### Members of the primary template

- ****(constructor)**** — constructs a `bad_expected_access` object (public
  member function)
- **error** — returns the stored value (public member function)
- **what** — returns the explanatory string (public member function)

## std::bad_expected_access::bad_expected_access

```cpp
explicit bad_expected_access( E e );
```

Constructs a new `bad_expected_access<E>` object. Initializes the stored value
with `std::move(e)`.

## std::bad_expected_access::error

```cpp
const E& error() const & noexcept;
E& error() & noexcept;
const E&& error() const && noexcept;
E&& error() && noexcept;
```

Returns a reference to the stored value.

## std::bad_expected_access::what

```cpp
const char* what() const noexcept override;
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

### Members of the `bad_expected_access<void>` specialization

- **(constructor)** — constructs a `bad_expected_access<void>` object (protected
  member function)
- **(destructor)** — destroys the `bad_expected_access<void>` object (protected
  member function)
- **operator=** — replaces the `bad_expected_access<void>` object (protected
  member function)
- **what** — returns the explanatory string (public member function)

Special member functions of `bad_expected_access<void>` are protected. They can
only be called by derived classes.

### Example

---
*Source: https://en.cppreference.com/w/cpp/utility/expected/bad_expected_access*
