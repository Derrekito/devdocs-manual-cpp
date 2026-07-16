# std::bad_cast

```cpp
class bad_cast : public std::exception;
```

An exception of this type is thrown when a `dynamic_cast` to a reference type
fails the run-time check (e.g. because the types are not related by
inheritance), and also from `std::use_facet` if the requested facet does not
exist in the locale.

### Member functions

- **(constructor)** — constructs a new `bad_cast` object (public member
  function)
- **operator=** — replaces the `bad_cast` object (public member function)
- **what** — returns the explanatory string (public member function)

## std::bad_cast::bad_cast

```cpp
bad_cast() throw();  // (until C++11)
bad_cast() noexcept;  // (since C++11)
bad_cast( const bad_cast& other ) throw();  // (until C++11)
bad_cast( const bad_cast& other ) noexcept;  // (since C++11)
```

Constructs a new `bad_cast` object with an implementation-defined
null-terminated byte string which is accessible through `what()`.

1) Default constructor.

2) Copy constructor. If `*this` and `other` both have dynamic type
   `std::bad_cast` then `std::strcmp(what(), other.what()) == 0`.(since C++11)

### Parameters

- **other** — another exception object to copy

## std::bad_cast::operator=

```cpp
bad_cast& operator=( const bad_cast& other ) throw();  // (until C++11)
bad_cast& operator=( const bad_cast& other ) noexcept;  // (since C++11)
```

Assigns the contents with those of `other`. If `*this` and `other` both have
dynamic type `std::bad_cast` then `std::strcmp(what(), other.what()) == 0` after
assignment.(since C++11)

### Parameters

- **other** — another exception object to assign with

### Return value

`*this`

## std::bad_cast::what

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
#include <typeinfo>

struct Foo { virtual ~Foo() {} };
struct Bar { virtual ~Bar() { std::cout << "~Bar\n"; } };
struct Pub : Bar { ~Pub() override { std::cout << "~Pub\n"; } };

int main()
{
    Pub pub;
    try
    {
        [[maybe_unused]]
        Bar& r1 = dynamic_cast<Bar&>(pub); // OK, upcast

        [[maybe_unused]]
        Foo& r2 = dynamic_cast<Foo&>(pub); // throws
    }
    catch (const std::bad_cast& e)
    {
        std::cout << "e.what(): " << e.what() << '\n';
    }
}
```

Possible output:

```text
e.what(): std::bad_cast
~Pub
~Bar
```

---
*Source: https://en.cppreference.com/w/cpp/types/bad_cast*
