# std::runtime_error

```cpp
class runtime_error;
```

Defines a type of object to be thrown as exception. It reports errors that are
due to events beyond the scope of the program and can not be easily predicted.

Exceptions of type `std::runtime_error` are thrown by the following standard
library components: `std::locale::locale` and `std::locale::combine`.

In addition, the following standard exception types are derived from
`std::runtime_error`:

- `std::range_error`
- `std::overflow_error`
- `std::underflow_error`

- `std::regex_error`
- `std::system_error`
*(since C++11)*

- `std::chrono::ambiguous_local_time`
- `std::chrono::nonexistent_local_time`
- `std::format_error`
*(since C++20)*

### Member functions

- **(constructor)** — constructs a new `runtime_error` object with the given
  message (public member function)
- **operator=** — replaces the `runtime_error` object (public member function)

## std::runtime_error::runtime_error

```cpp
runtime_error( const std::string& what_arg );  // (1)
runtime_error( const char* what_arg );  // (2)
runtime_error( const runtime_error& other );  // (until C++11)
runtime_error( const runtime_error& other ) noexcept;  // (since C++11)
```

1) Constructs the exception object with `what_arg` as explanatory string. After
   construction, `std::strcmp(what(), what_arg.c_str()) == 0`.

2) Constructs the exception object with `what_arg` as explanatory string. After
   construction, `std::strcmp(what(), what_arg) == 0`.

3) Copy constructor. If `*this` and `other` both have dynamic type
   `std::runtime_error` then `std::strcmp(what(), other.what()) == 0`. No
   exception can be thrown from the copy constructor.(until C++11)

### Parameters

- **what_arg** — explanatory string
- **other** — another exception object to copy

### Exceptions

1,2) May throw `std::bad_alloc`.

### Notes

Because copying `std::runtime_error` is not permitted to throw exceptions, this
message is typically stored internally as a separately-allocated
reference-counted string. This is also why there is no constructor taking
`std::string&&`: it would have to copy the content anyway.

Before the resolution of LWG issue 254, the non-copy constructor can only accept
`std::string`. It makes dynamic allocation mandatory in order to construct a
`std::string` object.

After the resolution of LWG issue 471, a derived standard exception class must
have a publicly accessible copy constructor. It can be implicitly defined as
long as the explanatory strings obtained by `what()` are the same for the
original object and the copied object.

## std::runtime_error::operator=

```cpp
runtime_error& operator=( const runtime_error& other );  // (until C++11)
runtime_error& operator=( const runtime_error& other ) noexcept;  // (since C++11)
```

Assigns the contents with those of `other`. If `*this` and `other` both have
dynamic type `std::runtime_error` then `std::strcmp(what(), other.what()) == 0`
after assignment. No exception can be thrown from the copy assignment
operator.(until C++11)

### Parameters

- **other** — another exception object to assign with

### Return value

`*this`

### Notes

After the resolution of LWG issue 471, a derived standard exception class must
have a publicly accessible copy assignment operator. It can be implicitly
defined as long as the explanatory strings obtained by `what()` are the same for
the original object and the copied object.

## Inherited from std::exception

### Member functions

- **(destructor) [virtual]** — destroys the exception object (virtual public
  member function of `std::exception`)
- **what [virtual]** — returns an explanatory string (virtual public member
  function of `std::exception`)

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 254 | C++98 | the constructor accepting `const char*` was missing | added
  LWG 471 | C++98 | the explanatory strings of `std::runtime_error`'s copies
      were implementation-defined | they are the same as that of the original
      `std::runtime_error` object

---
*Source: https://en.cppreference.com/w/cpp/error/runtime_error*
