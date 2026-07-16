# std::chrono::is_clock

```cpp
template< class T >
struct is_clock;  // (since C++20)
```

If `T` satisfies the Clock requirements, provides the member constant `value`
equal `true`. For any other type, `value` is `false`.

For the purpose of this trait, the extent to which an implementation determines
that a type cannot meet the Clock requirements is unspecified, except that a
minimum `T` shall not qualify as a Clock unless it meets all of the following
conditions:

- The *qualified-id*s `T::rep`, `T::period`, `T::duration`, and `T::time_point`
  are all valid and each denotes a type;
- The expressions `T::is_steady` and `T::now()` are each well-formed when
  treated as an unevaluated operand.

The behavior of a program that adds specializations for `std::is_clock` or
`std::is_clock_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_clock_v = is_clock<T>::value;  // (since C++20)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` satisfies the Clock requirements, `false`
  otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Possible implementation

```cpp
template<class>
struct is_clock : std::false_type {};

template<class T>
    requires
        requires {
            typename T::rep;
            typename T::period;
            typename T::duration;
            typename T::time_point;
            T::is_steady;
            T::now();
        }
struct is_clock<T> : std::true_type {};
```

### Example

```cpp
#include <chrono>
#include <ratio>

int main()
{
    static_assert
    (
        std::chrono::is_clock_v<std::chrono::utc_clock>
        and
        not std::chrono::is_clock_v<std::chrono::duration<int, std::exa>>
    );
}
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/is_clock*
