# std::length_error

```cpp
class length_error;
```

Defines a type of object to be thrown as exception. It reports errors that
result from attempts to exceed implementation defined length limits for some
object.

This exception is thrown by member functions of `std::basic_string` and
`std::vector::reserve`.

### Member functions

- **(constructor)** — constructs a new `length_error` object with the given
  message (public member function)
- **operator=** — replaces the `length_error` object (public member function)

## std::length_error::length_error

```cpp
length_error( const std::string& what_arg );  // (1)
length_error( const char* what_arg );  // (2)
length_error( const length_error& other );  // (until C++11)
length_error( const length_error& other ) noexcept;  // (since C++11)
```

1) Constructs the exception object with `what_arg` as explanatory string. After
   construction, `std::strcmp(what(), what_arg.c_str()) == 0`.

2) Constructs the exception object with `what_arg` as explanatory string. After
   construction, `std::strcmp(what(), what_arg) == 0`.

3) Copy constructor. If `*this` and `other` both have dynamic type
   `std::length_error` then `std::strcmp(what(), other.what()) == 0`. No
   exception can be thrown from the copy constructor.(until C++11)

### Parameters

- **what_arg** — explanatory string
- **other** — another exception object to copy

### Exceptions

1,2) May throw `std::bad_alloc`.

### Notes

Because copying `std::length_error` is not permitted to throw exceptions, this
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

## std::length_error::operator=

```cpp
length_error& operator=( const length_error& other );  // (until C++11)
length_error& operator=( const length_error& other ) noexcept;  // (since C++11)
```

Assigns the contents with those of `other`. If `*this` and `other` both have
dynamic type `std::length_error` then `std::strcmp(what(), other.what()) == 0`
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

## Inherited from std::logic_error

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
  LWG 471 | C++98 | the explanatory strings of `std::length_error`'s copies were
      implementation-defined | they are the same as that of the original
      `std::length_error` object

### See also

- **resize** — changes the number of characters stored (public member function
  of `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/error/length_error*
