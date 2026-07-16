# std::bad_array_new_length

```cpp
class bad_array_new_length;  // (since C++11)
```

`std::bad_array_new_length` is the type of the object thrown as exceptions by
the new-expressions to report invalid array lengths if

1. Array length is negative,
1. Total size of the new array would exceed implementation-defined maximum
   value,
1. The number of initializer-clauses exceeds the number of elements to
   initialize.

Only the first array dimension may generate this exception; dimensions other
than the first are constant expressions and are checked at compile time.

### Member functions

- **(constructor)** — constructs a new `bad_array_new_length` object (public
  member function)
- **operator=** — replaces the `bad_array_new_length` object (public member
  function)
- **what** — returns the explanatory string (public member function)

## std::bad_array_new_length::bad_array_new_length

```cpp
bad_array_new_length() noexcept;  // (1) (since C++11)
bad_array_new_length( const bad_array_new_length& other ) noexcept;  // (2) (since C++11)
```

Constructs a new `bad_array_new_length` object with an implementation-defined
null-terminated byte string which is accessible through `what()`.

1) Default constructor.

2) Copy constructor. If `*this` and `other` both have dynamic type
   `std::bad_array_new_length` then `std::strcmp(what(), other.what()) == 0`.

### Parameters

- **other** — another exception object to copy

## std::bad_array_new_length::operator=

```cpp
bad_array_new_length& operator=( const bad_array_new_length& other ) noexcept;  // (since C++11)
```

Assigns the contents with those of `other`. If `*this` and `other` both have
dynamic type `std::bad_array_new_length` then `std::strcmp(what(), other.what())
== 0` after assignment.

### Parameters

- **other** — another exception object to assign with

### Return value

`*this`

## std::bad_array_new_length::what

```cpp
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

## Inherited from std::bad_alloc

## Inherited from std::exception

### Member functions

- **(destructor) [virtual]** — destroys the exception object (virtual public
  member function of `std::exception`)
- **what [virtual]** — returns an explanatory string (virtual public member
  function of `std::exception`)

### Example

Three conditions where `std::bad_array_new_length` should be thrown:

```cpp
#include <climits>
#include <iostream>
#include <new>

int main()
{
    try
    {
        int negative = -1;
        new int[negative];
    }
    catch (const std::bad_array_new_length &e)
    {
        std::cout << "1) " << e.what() << ": negative size\n";
    }

    try
    {
        int small = 1;
        new int[small]{1,2,3};
    }
    catch (const std::bad_array_new_length &e)
    {
        std::cout << "2) " << e.what() << ": too many initializers\n";
    }

    try
    {
        long large = LONG_MAX;
        new int[large][1000];
    }
    catch (const std::bad_array_new_length &e)
    {
        std::cout << "3) " << e.what() << ": too large\n";
    }

    std::cout << "End\n";
}
```

Possible output:

```text
1) std::bad_array_new_length: negative size
2) std::bad_array_new_length: too many initializers
3) std::bad_array_new_length: too large
End
```

### See also

- **operator newoperator new[]** — allocation functions (function)
- **bad_alloc** — exception thrown when memory allocation fails (class)

---
*Source: https://en.cppreference.com/w/cpp/memory/new/bad_array_new_length*
