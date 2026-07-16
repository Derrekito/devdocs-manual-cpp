# std::bad_alloc

```cpp
class bad_alloc;
```

`std::bad_alloc` is the type of the object thrown as exceptions by the
allocation functions to report failure to allocate storage.

### Member functions

- **(constructor)** — constructs a new `bad_alloc` object (public member
  function)
- **operator=** — replaces the `bad_alloc` object (public member function)
- **what** — returns the explanatory string (public member function)

## std::bad_alloc::bad_alloc

```cpp
bad_alloc() throw();  // (until C++11)
bad_alloc() noexcept;  // (since C++11)
bad_alloc( const bad_alloc& other ) throw();  // (until C++11)
bad_alloc( const bad_alloc& other ) noexcept;  // (since C++11)
```

Constructs a new `bad_alloc` object with an implementation-defined
null-terminated byte string which is accessible through `what()`.

1) Default constructor.

2) Copy constructor. If `*this` and `other` both have dynamic type
   `std::bad_alloc` then `std::strcmp(what(), other.what()) == 0`.(since C++11)

### Parameters

- **other** — another exception object to copy

## std::bad_alloc::operator=

```cpp
bad_alloc& operator=( const bad_alloc& other ) throw();  // (until C++11)
bad_alloc& operator=( const bad_alloc& other ) noexcept;  // (since C++11)
```

Assigns the contents with those of `other`. If `*this` and `other` both have
dynamic type `std::bad_alloc` then `std::strcmp(what(), other.what()) == 0`
after assignment.(since C++11)

### Parameters

- **other** — another exception object to assign with

### Return value

`*this`

## std::bad_alloc::what

```cpp
virtual const char* what() const throw();  // (until C++11)
virtual const char* what() const noexcept;  // (since C++11)
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

### Example

```cpp
#include <iostream>
#include <new>

int main()
{
    try
    {
        while (true)
        {
            new int[100000000ul];
        }
    }
    catch (const std::bad_alloc& e)
    {
        std::cout << "Allocation failed: " << e.what() << '\n';
    }
}
```

Possible output:

```text
Allocation failed: std::bad_alloc
```

### See also

- **operator newoperator new[]** — allocation functions (function)

---
*Source: https://en.cppreference.com/w/cpp/memory/new/bad_alloc*
