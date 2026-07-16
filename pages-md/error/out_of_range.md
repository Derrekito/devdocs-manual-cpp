# std::out_of_range

```cpp
class out_of_range;
```

Defines a type of object to be thrown as exception. It reports errors that are
consequence of attempt to access elements out of defined range.

It may be thrown by the member functions of `std::bitset` and
`std::basic_string`, by `std::stoi` and `std::stod` families of functions, and
by the bounds-checked member access functions (e.g. `std::vector::at` and
`std::map::at`).

### Member functions

- **(constructor)** — constructs a new `out_of_range` object with the given
  message (public member function)
- **operator=** — replaces the `out_of_range` object (public member function)

## std::out_of_range::out_of_range

```cpp
out_of_range( const std::string& what_arg );  // (1)
out_of_range( const char* what_arg );  // (2)
out_of_range( const out_of_range& other );  // (until C++11)
out_of_range( const out_of_range& other ) noexcept;  // (since C++11)
```

1) Constructs the exception object with `what_arg` as explanatory string. After
   construction, `std::strcmp(what(), what_arg.c_str()) == 0`.

2) Constructs the exception object with `what_arg` as explanatory string. After
   construction, `std::strcmp(what(), what_arg) == 0`.

3) Copy constructor. If `*this` and `other` both have dynamic type
   `std::out_of_range` then `std::strcmp(what(), other.what()) == 0`. No
   exception can be thrown from the copy constructor.(until C++11)

### Parameters

- **what_arg** — explanatory string
- **other** — another exception object to copy

### Exceptions

1,2) May throw `std::bad_alloc`.

### Notes

Because copying `std::out_of_range` is not permitted to throw exceptions, this
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

## std::out_of_range::operator=

```cpp
out_of_range& operator=( const out_of_range& other );  // (until C++11)
out_of_range& operator=( const out_of_range& other ) noexcept;  // (since C++11)
```

Assigns the contents with those of `other`. If `*this` and `other` both have
dynamic type `std::out_of_range` then `std::strcmp(what(), other.what()) == 0`
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

### Notes

The standard error condition `std::errc::result_out_of_range` typically
indicates the condition where the result, rather than the input, is out of
range, and is more closely related to `std::range_error` and `ERANGE`.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 254 | C++98 | the constructor accepting `const char*` was missing | added
  LWG 471 | C++98 | the explanatory strings of `std::out_of_range`'s copies were
      implementation-defined | they are the same as that of the original
      `std::out_of_range` object

### See also

- **at** — accesses the specified character with bounds checking (public member
  function of `std::basic_string<CharT,Traits,Allocator>`)
- **at** — accesses the specified character with bounds checking (public member
  function of `std::basic_string_view<CharT,Traits>`)
- **at** — access specified element with bounds checking (public member function
  of `std::deque<T,Allocator>`)
- **at** — access specified element with bounds checking (public member function
  of `std::map<Key,T,Compare,Allocator>`)
- **at** — access specified element with bounds checking (public member function
  of `std::unordered_map<Key,T,Hash,KeyEqual,Allocator>`)
- **at** — access specified element with bounds checking (public member function
  of `std::vector<T,Allocator>`)
- **at** — access specified element with bounds checking (public member function
  of `std::array<T,N>`)
- **at (C++26)** — access specified element with bounds checking (public member
  function of `std::span<T,Extent>`)

---
*Source: https://en.cppreference.com/w/cpp/error/out_of_range*
