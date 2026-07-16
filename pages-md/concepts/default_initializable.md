# std::default_initializable

```cpp
template< class T >
concept default_initializable = std::constructible_from<T> && requires { T{}; } &&
                                /* T t; is well-formed, see below */;  // (since C++20)
```

The `default_initializable` concept checks whether variables of type `T` can be

- value-initialized (`T()` is well-formed);
- direct-list-initialized from an empty initializer list (`T{}` is well-formed);
  and
- default-initialized (`T t;` is well-formed).

Access checking is performed as if in a context unrelated to T. Only the
validity of the immediate context of the variable initialization is considered.

### Possible implementation

```cpp
template<class T>
concept default_initializable =
    std::constructible_from<T> &&
    requires { T{}; } &&
    requires { ::new T; };
```

### See also

- **constructible_from (C++20)** — specifies that a variable of the type can be
  constructed from or bound to a set of argument types (concept)
-
  **is_default_constructibleis_trivially_default_constructibleis_nothrow_default_constructible
  (C++11)(C++11)(C++11)** — checks if a type has a default constructor (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/concepts/default_initializable*
