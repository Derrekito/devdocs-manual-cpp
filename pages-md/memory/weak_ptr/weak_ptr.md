# std::weak_ptr<T>::weak_ptr

```cpp
constexpr weak_ptr() noexcept;  // (1) (since C++11)
weak_ptr( const weak_ptr& r ) noexcept;  // (2) (since C++11)
template< class Y >
weak_ptr( const weak_ptr<Y>& r ) noexcept;  // (2) (since C++11)
template< class Y >
weak_ptr( const std::shared_ptr<Y>& r ) noexcept;  // (2) (since C++11)
weak_ptr( weak_ptr&& r ) noexcept;  // (3) (since C++11)
template< class Y >
weak_ptr( weak_ptr<Y>&& r ) noexcept;  // (3) (since C++11)
```

Constructs new `weak_ptr` that potentially shares an object with `r`.

1) Default constructor. Constructs empty `weak_ptr`.

2) Constructs new `weak_ptr` which shares an object managed by `r`. If `r`
   manages no object, `*this` manages no object too. The templated overloads
   don't participate in the overload resolution unless `Y*` is implicitly
   convertible to `T*`, or `Y` is the type "array of `N` `U`" for some type `U`
   and some number `N`, and `T` is the type "array of unknown bound of (possibly
   cv-qualified) `U`"(since C++17).

3) Move constructors. Moves a weak_ptr instance from `r` into `*this`. After
   this, `r` is empty and `r.use_count() == 0`. The templated overload doesn't
   participate in the overload resolution unless `Y*` is implicitly convertible
   to `T*`.

### Parameters

- **r** — a `std::shared_ptr` or `std::weak_ptr` that will be viewed by this
  `std::weak_ptr`

### Notes

Because the default constructor is `constexpr`, static `std::weak_ptr`s are
initialized as part of static non-local initialization, before any dynamic
non-local initialization begins. This makes it safe to use a `std::weak_ptr` in
a constructor of any static object.

### Example

```cpp
#include <iostream>
#include <memory>

struct Foo {};

int main()
{
    std::weak_ptr<Foo> w_ptr;

    {
        auto ptr = std::make_shared<Foo>();
        w_ptr = ptr;
        std::cout << "w_ptr.use_count() inside scope: " << w_ptr.use_count() << '\n';
    }

    std::cout << "w_ptr.use_count() out of scope: " << w_ptr.use_count() << '\n';
    std::cout << "w_ptr.expired() out of scope: "
              << std::boolalpha << w_ptr.expired() << '\n';
}
```

Output:

```text
w_ptr.use_count() inside scope: 1
w_ptr.use_count() out of scope: 0
w_ptr.expired() out of scope: true
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2315 | C++11 | move semantic was not enabled for `weak_ptr` | enabled

### See also

- **operator=** — assigns the `weak_ptr` (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/weak_ptr/weak_ptr*
