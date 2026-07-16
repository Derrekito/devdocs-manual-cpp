# std::exception_ptr

```cpp
typedef /*unspecified*/ exception_ptr;  // (since C++11)
```

`std::exception_ptr` is a nullable pointer-like type that manages an exception
object which has been thrown and captured with `std::current_exception`. An
instance of `std::exception_ptr` may be passed to another function, possibly on
another thread, where the exception may be rethrown and handled with a catch
clause.

A default-constructed `std::exception_ptr` is a null pointer; it does not point
to an exception object.

Two instances of `std::exception_ptr` compare equal only if they are both null
or both point at the same exception object.

`std::exception_ptr` is not implicitly convertible to any arithmetic,
enumeration, or pointer type. It is contextually convertible to `bool`, and will
evaluate to false if it is null, true otherwise.

The exception object referenced by an `std::exception_ptr` remains valid as long
as there remains at least one `std::exception_ptr` that is referencing it:
`std::exception_ptr` is a shared-ownership smart pointer (note: this is in
addition to the usual exception object lifetime rules).

`std::exception_ptr` meets the requirements of NullablePointer.

### Example

```cpp
#include <exception>
#include <iostream>
#include <stdexcept>
#include <string>

void handle_eptr(std::exception_ptr eptr) // passing by value is ok
{
    try
    {
        if (eptr)
            std::rethrow_exception(eptr);
    }
    catch(const std::exception& e)
    {
        std::cout << "Caught exception: '" << e.what() << "'\n";
    }
}

int main()
{
    std::exception_ptr eptr;

    try
    {
        std::string().at(1); // this generates a std::out_of_range
    }
    catch(...)
    {
        eptr = std::current_exception(); // capture
    }

    handle_eptr(eptr);

} // destructor for std::out_of_range called here, when the eptr is destructed
```

Possible output:

```text
Caught exception: 'basic_string::at: __n (which is 1) >= this->size() (which is 0)'
```

### See also

- **make_exception_ptr (C++11)** — creates an `std::exception_ptr` from an
  exception object (function template)
- **current_exception (C++11)** — captures the current exception in a
  `std::exception_ptr` (function)
- **rethrow_exception (C++11)** — throws the exception from an
  `std::exception_ptr` (function)

---
*Source: https://en.cppreference.com/w/cpp/error/exception_ptr*
