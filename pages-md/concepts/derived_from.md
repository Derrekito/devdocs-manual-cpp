# std::derived_from

```cpp
template< class Derived, class Base >
concept derived_from =
    std::is_base_of_v<Base, Derived> &&
    std::is_convertible_v<const volatile Derived*, const volatile Base*>;  // (since C++20)
```

The concept `derived_from<Derived, Base>` is satisfied if and only if `Base` is
a class type that is either `Derived` or a public and unambiguous base of
`Derived`, ignoring cv-qualifiers.

Note that this behavior is different to `std::is_base_of` when `Base` is a
private or protected base of `Derived`.

### Example

```cpp
#include <concepts>

class A {};

class B : public A {};

class C : private A {};

// std::derived_from == true only for public inheritance or exact same class
static_assert(std::derived_from<B, B> == true);      // same class: true
static_assert(std::derived_from<int, int> == false); // same primitive type: false
static_assert(std::derived_from<B, A> == true);      // public inheritance: true
static_assert(std::derived_from<C, A> == false);     // private inheritance: false

// std::is_base_of == true also for private inheritance
static_assert(std::is_base_of_v<B, B> == true);      // same class: true
static_assert(std::is_base_of_v<int, int> == false); // same primitive type: false
static_assert(std::is_base_of_v<A, B> == true);      // public inheritance: true
static_assert(std::is_base_of_v<A, C> == true);      // private inheritance: true

int main() {}
```

### See also

- **is_base_of (C++11)** — checks if a type is derived from the other type
  (class template)
- **is_convertibleis_nothrow_convertible (C++11)(C++20)** — checks if a type can
  be converted to the other type (class template)

---
*Source: https://en.cppreference.com/w/cpp/concepts/derived_from*
