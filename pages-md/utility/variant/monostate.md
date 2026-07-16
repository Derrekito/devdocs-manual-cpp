# std::monostate

```cpp
struct monostate { };  // (since C++17)
```

Unit type intended for use as a well-behaved empty alternative in
`std::variant`. In particular, a variant of non-default-constructible types may
list `std::monostate` as its first alternative: this makes the variant itself
default-constructible.

### Member functions

- **(constructor) (implicitly declared)** — trivial implicit default/copy/move
  constructor (public member function)
- **(destructor) (implicitly declared)** — trivial implicit destructor (public
  member function)
- **operator= (implicitly declared)** — trivial implicit copy/move assignment
  (public member function)

### Non-member functions

## std::operator==, !=, <, <=, >, >=, <=>(std::monostate)

```cpp
constexpr bool operator==( monostate, monostate ) noexcept { return true; }  // (1) (since C++17)
constexpr bool operator!=( monostate, monostate ) noexcept { return false; }
constexpr bool operator< ( monostate, monostate ) noexcept { return false; }
constexpr bool operator> ( monostate, monostate ) noexcept { return false; }
constexpr bool operator<=( monostate, monostate ) noexcept { return true; }
constexpr bool operator>=( monostate, monostate ) noexcept { return true; }  // (since C++17) (until C++20)
constexpr std::strong_ordering operator<=>( monostate, monostate ) noexcept
{
    return std::strong_ordering::equal;
}  // (since C++20)
```

All instances of `std::monostate` compare equal.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.
*(since C++20)*

### Helper classes

## std::hash<std::monostate>

```cpp
template <>
struct std::hash<monostate>;  // (since C++17)
```

Specializes the `std::hash` algorithm for `std::monostate`.

### Example

```cpp
#include <cassert>
#include <iostream>
#include <variant>

struct S
{
    S(int i) : i(i) {}
    int i;
};

int main()
{
    // Without the monostate type this declaration will fail.
    // This is because S is not default-constructible.
    std::variant<std::monostate, S> var;
    assert(var.index() == 0);

    try
    {
        std::get<S>(var); // throws! We need to assign a value
    }
    catch(const std::bad_variant_access& e)
    {
        std::cout << e.what() << '\n';
    }

    var = 42;
    std::cout << "std::get: " << std::get<S>(var).i << '\n'
              << "std::hash: " << std::hex << std::showbase
              << std::hash<std::monostate>{}(std::monostate{}) << '\n';
}
```

Possible output:

```text
std::get: wrong index for variant
std::get: 42
std::hash: 0xffffffffffffe19f
```

### See also

- **(constructor)** — constructs the variant object (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/variant/monostate*
