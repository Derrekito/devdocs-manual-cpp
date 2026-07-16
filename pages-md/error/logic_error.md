# std::logic_error

```cpp
class logic_error;
```

Defines a type of object to be thrown as exception. It reports errors that are a
consequence of faulty logic within the program such as violating logical
preconditions or class invariants and may be preventable.

No standard library components throw this exception directly, but the exception
types `std::invalid_argument`, `std::domain_error`, `std::length_error`,
`std::out_of_range`, `std::future_error`, and
`std::experimental::bad_optional_access` are derived from `std::logic_error`.

### Member functions

- **(constructor)** — constructs a new `logic_error` object with the given
  message (public member function)
- **operator=** — replaces the `logic_error` object (public member function)

## std::logic_error::logic_error

```cpp
logic_error( const std::string& what_arg );  // (1)
logic_error( const char* what_arg );  // (2)
logic_error( const logic_error& other );  // (until C++11)
logic_error( const logic_error& other ) noexcept;  // (since C++11)
```

1) Constructs the exception object with `what_arg` as explanatory string. After
   construction, `std::strcmp(what(), what_arg.c_str()) == 0`.

2) Constructs the exception object with `what_arg` as explanatory string. After
   construction, `std::strcmp(what(), what_arg) == 0`.

3) Copy constructor. If `*this` and `other` both have dynamic type
   `std::logic_error` then `std::strcmp(what(), other.what()) == 0`. No
   exception can be thrown from the copy constructor.(until C++11)

### Parameters

- **what_arg** — explanatory string
- **other** — another exception object to copy

### Exceptions

1,2) May throw `std::bad_alloc`.

### Notes

Because copying `std::logic_error` is not permitted to throw exceptions, this
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

## std::logic_error::operator=

```cpp
logic_error& operator=( const logic_error& other );  // (until C++11)
logic_error& operator=( const logic_error& other ) noexcept;  // (since C++11)
```

Assigns the contents with those of `other`. If `*this` and `other` both have
dynamic type `std::logic_error` then `std::strcmp(what(), other.what()) == 0`
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
  LWG 471 | C++98 | the explanatory strings of `std::logic_error`'s copies were
      implementation-defined | they are the same as that of the original
      `std::logic_error` object

---
*Source: https://en.cppreference.com/w/cpp/error/logic_error*
