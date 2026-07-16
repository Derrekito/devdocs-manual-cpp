# std::mem_fn

```cpp
template< class M, class T >
/* unspecified */ mem_fn( M T::* pm ) noexcept;  // (since C++11) (until C++20)
template< class M, class T >
constexpr /* unspecified */ mem_fn( M T::* pm ) noexcept;  // (since C++20)
```

Function template `std::mem_fn` generates wrapper objects for pointers to
members, which can store, copy, and invoke a pointer to member. Both references
and pointers (including smart pointers) to an object can be used when invoking a
`std::mem_fn`.

### Parameters

- **pm** — pointer to member that will be wrapped

### Return value

`std::mem_fn` returns a call wrapper `fn` of unspecified type that has the
following members:

## std::mem_fn *return type*

### Member types
- **`result_type`(deprecated in C++17)** — the return type of `pm` if `pm` is a
  pointer to member function, not defined for pointer to member object
- **`argument_type`(deprecated in C++17)** — `T*`, possibly cv-qualified, if
  `pm` is a pointer to member function taking no arguments
- **`first_argument_type`(deprecated in C++17)** — `T*` if `pm` is a pointer to
  member function taking one argument
- **`second_argument_type`(deprecated in C++17)** — `T1` if `pm` is a pointer to
  member function taking one argument of type `T1`
*(until C++20)*

### Member function

```cpp
template< class... Args >
/* see below */ operator()(Args&&... args) /* cvref-qualifiers */
    noexcept(/* see below */);  // (until C++20)
template< class... Args >
constexpr /* see below */ operator()(Args&&... args) /* cvref-qualifiers */
    noexcept(/* see below */);  // (since C++20)
```

The expression `fn(args)` is equivalent to `INVOKE(pmd, args)`, where `pmd` is
the Callable object held by `fn`, it is of type `M T::*` and is
direct-non-list-initialized with `pm`.

Thus, the return type of operator() is
std::result_of<decltype(pm)(Args&&...)>::type or equivalently
`std::invoke_result_t<decltype(pm), Args&&...>`, and the value in `noexcept`
specifier is equal to `std::is_nothrow_invocable_v<decltype(pm),
Args&&...>)`(since C++17).

Each argument in `args` is perfectly forwarded, as if by
`std::forward<Args>(args)...`.

### Example

Use `std::mem_fn` to store and execute a member function and a member object:

```cpp
#include <functional>
#include <iostream>
#include <memory>

struct Foo
{
    void display_greeting()
    {
        std::cout << "Hello, world.\n";
    }

    void display_number(int i)
    {
        std::cout << "number: " << i << '\n';
    }

    int add_xy(int x, int y)
    {
        return data + x + y;
    }

    template<typename... Args> int add_many(Args... args)
    {
        return data + (args + ...);
    }

    auto add_them(auto... args) // C++20 required
    {
        return data + (args + ...);
    }

    int data = 7;
};

int main()
{
    auto f = Foo{};

    auto greet = std::mem_fn(&Foo::display_greeting);
    greet(f);

    auto print_num = std::mem_fn(&Foo::display_number);
    print_num(f, 42);

    auto access_data = std::mem_fn(&Foo::data);
    std::cout << "data: " << access_data(f) << '\n';

    auto add_xy = std::mem_fn(&Foo::add_xy);
    std::cout << "add_xy: " << add_xy(f, 1, 2) << '\n';

    auto u = std::make_unique<Foo>();
    std::cout << "access_data(u): " << access_data(u) << '\n';
    std::cout << "add_xy(u, 1, 2): " << add_xy(u, 1, 2) << '\n';

    auto add_many = std::mem_fn(&Foo::add_many<short, int, long>);
    std::cout << "add_many(u, ...): " << add_many(u, 1, 2, 3) << '\n';

    auto add_them = std::mem_fn(&Foo::add_them<short, int, float, double>);
    std::cout << "add_them(u, ...): " << add_them(u, 5, 7, 10.0f, 13.0) << '\n';
}
```

Output:

```text
Hello, world.
number: 42
data: 7
add_xy: 10
access_data(u): 7
add_xy(u, 1, 2): 10
add_many(u, ...): 13
add_them(u, ...): 42
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2048 | C++11 | unnecessary overloads provided | removed
  LWG 2489 | C++11 | noexcept not required | required

### See also

- **function (C++11)** — wraps callable object of any copy constructible type
  with specified function call signature (class template)
- **move_only_function (C++23)** — wraps callable object of any type with
  specified function call signature (class template)
- **bind (C++11)** — binds one or more arguments to a function object (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/mem_fn*
