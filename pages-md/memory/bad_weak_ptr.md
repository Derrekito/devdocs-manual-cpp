# std::bad_weak_ptr

```cpp
class bad_weak_ptr;  // (since C++11)
```

`std::bad_weak_ptr` is the type of the object thrown as exceptions by the
constructors of `std::shared_ptr` that take `std::weak_ptr` as the argument,
when the `std::weak_ptr` refers to an already deleted object.

### Member functions

- **(constructor)** — constructs a new `bad_weak_ptr` object (public member
  function)
- **operator=** — replaces the `bad_weak_ptr` object (public member function)
- **what** — returns the explanatory string (public member function)

## std::bad_weak_ptr::bad_weak_ptr

```cpp
bad_weak_ptr() noexcept;  // (1) (since C++11)
bad_weak_ptr( const bad_weak_ptr& other ) noexcept;  // (2) (since C++11)
```

Constructs a new `bad_weak_ptr` object with an implementation-defined
null-terminated byte string which is accessible through `what()`.

1) Default constructor.

2) Copy constructor. If `*this` and `other` both have dynamic type
   `std::bad_weak_ptr` then `std::strcmp(what(), other.what()) == 0`.

### Parameters

- **other** — another exception object to copy

## std::bad_weak_ptr::operator=

```cpp
bad_weak_ptr& operator=( const bad_weak_ptr& other ) noexcept;  // (since C++11)
```

Assigns the contents with those of `other`. If `*this` and `other` both have
dynamic type `std::bad_weak_ptr` then `std::strcmp(what(), other.what()) == 0`
after assignment.

### Parameters

- **other** — another exception object to assign with

### Return value

`*this`

## std::bad_weak_ptr::what

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

## Inherited from std::exception

### Member functions

- **(destructor) [virtual]** — destroys the exception object (virtual public
  member function of `std::exception`)
- **what [virtual]** — returns an explanatory string (virtual public member
  function of `std::exception`)

### Example

```cpp
#include <iostream>
#include <memory>

int main()
{
    std::shared_ptr<int> p1(new int(42));
    std::weak_ptr<int> wp(p1);
    p1.reset();
    try
    {
        std::shared_ptr<int> p2(wp);
    }
    catch (const std::bad_weak_ptr& e)
    {
        std::cout << e.what() << '\n';
    }
}
```

Possible output:

```text
std::bad_weak_ptr
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2376 | C++11 | calling `what` on a default-constructed `bad_weak_ptr` was
      required to return `"bad_weak_ptr"` | the return value is
      implementation-defined

### See also

- **shared_ptr (C++11)** — smart pointer with shared object ownership semantics
  (class template)
- **weak_ptr (C++11)** — weak reference to an object managed by
  `std::shared_ptr` (class template)

---
*Source: https://en.cppreference.com/w/cpp/memory/bad_weak_ptr*
