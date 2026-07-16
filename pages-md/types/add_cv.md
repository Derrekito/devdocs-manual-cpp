# std::add_cv, std::add_const, std::add_volatile

```cpp
template< class T >
struct add_cv;  // (1) (since C++11)
template< class T >
struct add_const;  // (2) (since C++11)
template< class T >
struct add_volatile;  // (3) (since C++11)
```

Provides the member typedef `type` which is the same as `T`, except it has a
cv-qualifier added (unless `T` is a function, a reference, or already has this
cv-qualifier)

1) adds both `const` and `volatile`

2) adds `const`

3) adds `volatile`

The behavior of a program that adds specializations for any of the templates
described on this page is undefined.

### Member types

- **`type`** — the type `T` with the cv-qualifier

### Helper types

```cpp
template< class T >
using add_cv_t       = typename add_cv<T>::type;  // (since C++14)
template< class T >
using add_const_t    = typename add_const<T>::type;  // (since C++14)
template< class T >
using add_volatile_t = typename add_volatile<T>::type;  // (since C++14)
```

### Possible implementation

```cpp
template<class T> struct add_cv { typedef const volatile T type; };

template<class T> struct add_const { typedef const T type; };

template<class T> struct add_volatile { typedef volatile T type; };
```

### Notes

These transformation traits can be used to establish non-deduced contexts in
template argument deduction:

```cpp
template<class T>
void f(const T&, const T&);

template<class T>
void g(const T&, std::add_const_t<T>&);

f(4.2, 0); // error, deduced conflicting types for 'T'
g(4.2, 0); // OK, calls g<double>
```

### Example

```cpp
#include <iostream>
#include <type_traits>

struct foo
{
    void m() { std::cout << "Non-cv\n"; }
    void m() const { std::cout << "Const\n"; }
    void m() volatile { std::cout << "Volatile\n"; }
    void m() const volatile { std::cout << "Const-volatile\n"; }
};

int main()
{
    foo{}.m();
    std::add_const<foo>::type{}.m();
    std::add_volatile<foo>::type{}.m();
    std::add_cv<foo>::type{}.m();
}
```

Output:

```text
Non-cv
Const
Volatile
Const-volatile
```

### See also

- **is_const (C++11)** — checks if a type is const-qualified (class template)
- **is_volatile (C++11)** — checks if a type is volatile-qualified (class
  template)
- **remove_cvremove_constremove_volatile (C++11)(C++11)(C++11)** — removes const
  and/or volatile specifiers from the given type (class template)
- **as_const (C++17)** — obtains a reference to const to its argument (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/types/add_cv*
