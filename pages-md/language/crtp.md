# Curiously Recurring Template Pattern

The Curiously Recurring Template Pattern is an idiom in which a class `X`
derives from a class template `Y`, taking a template parameter `Z`, where `Y` is
instantiated with `Z = X`. For example,

```cpp
template<class Z>
class Y {};

class X : public Y<X> {};
```

### Example

CRTP may be used to implement "compile-time polymorphism", when a base class
exposes an interface, and derived classes implement such interface.

```cpp
#include <cstdio>

#ifndef __cpp_explicit_this_parameter // Traditional syntax

template <class Derived>
struct Base { void name() { (static_cast<Derived*>(this))->impl(); } };
struct D1 : public Base<D1> { void impl() { std::puts("D1::impl()"); } };
struct D2 : public Base<D2> { void impl() { std::puts("D2::impl()"); } };

void test()
{
    // Base<D1> b1; b1.name(); //undefined behavior
    // Base<D2> b2; b2.name(); //undefined behavior
    D1 d1; d1.name();
    D2 d2; d2.name();
}

#else // C++23 alternative syntax; https://godbolt.org/z/s1o6qTMnP

struct Base { void name(this auto&& self) { self.impl(); } };
struct D1 : public Base { void impl() { std::puts("D1::impl()"); } };
struct D2 : public Base { void impl() { std::puts("D2::impl()"); } };

void test()
{
    D1 d1; d1.name();
    D2 d2; d2.name();
}

#endif

int main()
{
    test();
}
```

Output:

```text
D1::impl()
D2::impl()
```

### See also

- **enable_shared_from_this (C++11)** — allows an object to create a
  `shared_ptr` referring to itself (class template)
- **ranges::view_interface (C++20)** — helper class template for defining a
  `view`, using the **curiously recurring template pattern** (class template)

---
*Source: https://en.cppreference.com/w/cpp/language/crtp*
