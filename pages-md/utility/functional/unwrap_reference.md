# std::unwrap_reference, std::unwrap_ref_decay

```cpp
template< class T >
struct unwrap_reference;  // (1) (since C++20)
template< class T >
struct unwrap_ref_decay;  // (2) (since C++20)
```

1) If `T` is `std::reference_wrapper<U>` for some type `U`, provides a member
   type alias `type` that names `U&`; otherwise, provides a member type alias
   `type` that names `T`.

2) If `T` is `std::reference_wrapper<U>` for some type `U`, ignoring
   cv-qualification and referenceness, provides a member type alias `type` that
   names `U&`; otherwise, provides a member type alias `type` that names
   `std::decay_t<T>`.

The behavior of a program that adds specializations for any of the templates
described on this page is undefined.

### Member types

- **`type`** — (1) `U&` if `T` is `std::reference_wrapper<U>`; `T` otherwise (2)
  `U&` if `std::decay_t<T>` is `std::reference_wrapper<U>`; `std::decay_t<T>`
  otherwise

### Helper types

```cpp
template<class T>
using unwrap_reference_t = typename unwrap_reference<T>::type;  // (1) (since C++20)
template<class T>
using unwrap_ref_decay_t = typename unwrap_ref_decay<T>::type;  // (2) (since C++20)
```

### Possible implementation

```cpp
template<class T>
struct unwrap_reference { using type = T; };
template<class U>
struct unwrap_reference<std::reference_wrapper<U>> { using type = U&; };

template<class T>
struct unwrap_ref_decay : std::unwrap_reference<std::decay_t<T>> {};
```

### Notes

`std::unwrap_ref_decay` performs the same transformation as used by
`std::make_pair` and `std::make_tuple`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_unwrap_ref` | 201811L | (C++20) | `std::unwrap_ref_decay` and
      `std::unwrap_reference`

### Example

```cpp
#include <cassert>
#include <functional>
#include <iostream>
#include <type_traits>

int main()
{
    static_assert(std::is_same_v<std::unwrap_reference_t<int>, int>);
    static_assert(std::is_same_v<std::unwrap_reference_t<const int>, const int>);
    static_assert(std::is_same_v<std::unwrap_reference_t<int&>, int&>);
    static_assert(std::is_same_v<std::unwrap_reference_t<int&&>, int&&>);
    static_assert(std::is_same_v<std::unwrap_reference_t<int*>, int*>);

    {
        using T = std::reference_wrapper<int>;
        using X = std::unwrap_reference_t<T>;
        static_assert(std::is_same_v<X, int&>);
    }
    {
        using T = std::reference_wrapper<int&>;
        using X = std::unwrap_reference_t<T>;
        static_assert(std::is_same_v<X, int&>);
    }

    static_assert(std::is_same_v<std::unwrap_ref_decay_t<int>, int>);
    static_assert(std::is_same_v<std::unwrap_ref_decay_t<const int>, int>);
    static_assert(std::is_same_v<std::unwrap_ref_decay_t<const int&>, int>);

    {
        using T = std::reference_wrapper<int&&>;
        using X = std::unwrap_ref_decay_t<T>;
        static_assert(std::is_same_v<X, int&>);
    }

    {
        auto reset = []<typename T>(T&& z)
        {
        //  x = 0; // Error: does not work if T is reference_wrapper<>
            // converts T&& into T& for ordinary types
            // converts T&& into U& for reference_wrapper<U>
            decltype(auto) r = std::unwrap_reference_t<T>(z);
            std::cout << "r: " << r << '\n';
            r = 0; // OK, r has reference type
        };

        int x = 1;
        reset(x);
        assert(x == 0);

        int y = 2;
        reset(std::ref(y));
        assert(y == 0);
    }
}
```

Output:

```text
r: 1
r: 2
```

### See also

- **reference_wrapper (C++11)** — CopyConstructible and CopyAssignable reference
  wrapper (class template)
- **make_pair** — creates a `pair` object of type, defined by the argument types
  (function template)
- **make_tuple (C++11)** — creates a `tuple` object of the type defined by the
  argument types (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/unwrap_reference*
