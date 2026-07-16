# std::remove_cv, std::remove_const, std::remove_volatile

```cpp
template< class T >
struct remove_cv;  // (1) (since C++11)
template< class T >
struct remove_const;  // (2) (since C++11)
template< class T >
struct remove_volatile;  // (3) (since C++11)
```

Provides the member typedef `type` which is the same as `T`, except that its
topmost cv-qualifiers are removed.

1) Removes the topmost `const`, or the topmost `volatile`, or both, if present.

2) Removes the topmost `const`.

3) Removes the topmost `volatile`.

The behavior of a program that adds specializations for any of the templates
described on this page is undefined.

### Member types

- **`type`** — the type `T` without cv-qualifier

### Helper types

```cpp
template< class T >
using remove_cv_t = typename remove_cv<T>::type;  // (since C++14)
template< class T >
using remove_const_t = typename remove_const<T>::type;  // (since C++14)
template< class T >
using remove_volatile_t = typename remove_volatile<T>::type;  // (since C++14)
```

### Possible implementation

```cpp
template<class T> struct remove_cv { typedef T type; };
template<class T> struct remove_cv<const T> { typedef T type; };
template<class T> struct remove_cv<volatile T> { typedef T type; };
template<class T> struct remove_cv<const volatile T> { typedef T type; };

template<class T> struct remove_const { typedef T type; };
template<class T> struct remove_const<const T> { typedef T type; };

template<class T> struct remove_volatile { typedef T type; };
template<class T> struct remove_volatile<volatile T> { typedef T type; };
```

### Example

Removing const/volatile from `const volatile int*` does not modify the type,
because the pointer itself is neither const nor volatile.

```cpp
#include <type_traits>

template<typename U, typename V>
constexpr bool same = std::is_same_v<U, V>;

static_assert
(
    same<std::remove_cv_t<int>, int> &&
    same<std::remove_cv_t<const int>, int> &&
    same<std::remove_cv_t<volatile int>, int> &&
    same<std::remove_cv_t<const volatile int>, int> &&
    // remove_cv only works on types, not on pointers
    not same<std::remove_cv_t<const volatile int*>, int*> &&
    same<std::remove_cv_t<const volatile int*>, const volatile int*> &&
    same<std::remove_cv_t<const int* volatile>, const int*> &&
    same<std::remove_cv_t<int* const volatile>, int*>
);

int main() {}
```

### See also

- **is_const (C++11)** — checks if a type is const-qualified (class template)
- **is_volatile (C++11)** — checks if a type is volatile-qualified (class
  template)
- **add_cvadd_constadd_volatile (C++11)(C++11)(C++11)** — adds const and/or
  volatile specifiers to the given type (class template)
- **remove_cvref (C++20)** — combines `std::remove_cv` and
  `std::remove_reference` (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/remove_cv*
