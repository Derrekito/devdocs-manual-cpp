# std::chrono::ambiguous_local_time

```cpp
class ambiguous_local_time;  // (since C++20)
```

Defines a type of object to be thrown as exception to report that an attempt was
made to convert an ambiguous `std::chrono::local_time` to a
`std::chrono::sys_time` without specifying a `std::chrono::choose` (such as
`choose::earliest` or `choose::latest`).

This exception is thrown by `std::chrono::time_zone::to_sys` and functions that
call it (such as the constructors of `std::chrono::zoned_time` that take a
`std::chrono::local_time`).

### Member functions

- **(constructor)** — constructs the exception object (public member function)
- **operator=** — replaces the exception object (public member function)
- **what** — returns the explanatory string (public member function)

## std::chrono::ambiguous_local_time::ambiguous_local_time

```cpp
template< class Duration >
ambiguous_local_time( const std::chrono::local_time<Duration>& tp,
                      const std::chrono::local_info& i );  // (1) (since C++20)
ambiguous_local_time( const ambiguous_local_time& other ) noexcept;  // (2) (since C++20)
```

Constructs the exception object.

1) The explanatory string returned by `what()` is equivalent to that produced by
   `os.str()` after the following code:

```cpp
std::ostringstream os;
os << tp << " is ambiguous.  It could be\n"
   << tp << ' ' << i.first.abbrev << " == "
   << tp - i.first.offset << " UTC or\n"
   << tp << ' ' << i.second.abbrev  << " == "
   << tp - i.second.offset  << " UTC";
```

The behavior is undefined if `i.result != std::chrono::local_info::ambiguous`.

2) Copy constructor. If `*this` and `other` both have dynamic type
   `std::chrono::ambiguous_local_time` then `std::strcmp(what(), other.what())
   == 0`.

### Parameters

- **tp** — the time point for which conversion was attempted
- **i** — a `std::chrono::local_info` describing the result of the conversion
  attempt
- **other** — another `ambiguous_local_time` to copy

### Exceptions

May throw `std::bad_alloc`

### Notes

Because copying a standard library class derived from `std::exception` is not
permitted to throw exceptions, this message is typically stored internally as a
separately-allocated reference-counted string.

## std::chrono::ambiguous_locale_time::operator=

```cpp
ambiguous_locale_time& operator=( const ambiguous_locale_time& other ) noexcept;  // (since C++20)
```

Assigns the contents with those of `other`. If `*this` and `other` both have
dynamic type `std::chrono::ambiguous_locale_time` then `std::strcmp(what(),
other.what()) == 0` after assignment.

### Parameters

- **other** — another exception object to assign with

### Return value

`*this`

## std::chrono::ambiguous_locale_time::what

```cpp
virtual const char* what() const noexcept;  // (since C++20)
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

## Inherited from std::runtime_error

## Inherited from std::exception

### Member functions

- **(destructor) [virtual]** — destroys the exception object (virtual public
  member function of `std::exception`)
- **what [virtual]** — returns an explanatory string (virtual public member
  function of `std::exception`)

### See also

- **nonexistent_local_time (C++20)** — exception thrown to report that a local
  time is nonexistent (class)

---
*Source: https://en.cppreference.com/w/cpp/chrono/ambiguous_local_time*
