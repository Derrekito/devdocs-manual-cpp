# std::move_only_function

```cpp
template< class... >
class move_only_function; // not defined  // (1) (since C++23)
template< class R, class... Args >
class move_only_function<R(Args...)>;
template< class R, class... Args >
class move_only_function<R(Args...) noexcept>;
template< class R, class... Args >
class move_only_function<R(Args...) &>;
template< class R, class... Args >
class move_only_function<R(Args...) & noexcept>;
template< class R, class... Args >
class move_only_function<R(Args...) &&>;
template< class R, class... Args >
class move_only_function<R(Args...) && noexcept>;
template< class R, class... Args >
class move_only_function<R(Args...) const>;
template< class R, class... Args >
class move_only_function<R(Args...) const noexcept>;
template< class R, class... Args >
class move_only_function<R(Args...) const &>;
template< class R, class... Args >
class move_only_function<R(Args...) const & noexcept>;
template< class R, class... Args >
class move_only_function<R(Args...) const &&>;
template< class R, class... Args >
class move_only_function<R(Args...) const && noexcept>;  // (2) (since C++23)
```

Class template `std::move_only_function` is a general-purpose polymorphic
function wrapper. `std::move_only_function` objects can store and invoke any
constructible (not required to be move constructible) Callable *target* —
functions, lambda expressions, bind expressions, or other function objects, as
well as pointers to member functions and pointers to member objects.

The stored callable object is called the *target* of `std::move_only_function`.
If a `std::move_only_function` contains no target, it is called *empty*. Unlike
`std::function`, invoking an *empty* `std::move_only_function` results in
undefined behavior.

`std::move_only_function`s supports every possible combination of cv-qualifiers,
ref-qualifiers, and noexcept-specifiers not including `volatile` provided in its
template parameter. These qualifiers and specifier (if any) are added to its
`operator()`.

`std::move_only_function` satisfies the requirements of MoveConstructible and
MoveAssignable, but does not satisfy CopyConstructible or CopyAssignable.

### Member types

- **`result_type`** — `R`

### Member functions

- **(constructor) (C++23)** — constructs a new `std::move_only_function` object
  (public member function)
- **(destructor) (C++23)** — destroys a `std::move_only_function` object (public
  member function)
- **operator= (C++23)** — replaces or destroys the target (public member
  function)
- **swap (C++23)** — swaps the targets of two `std::move_only_function` objects
  (public member function)
- **operator bool (C++23)** — checks if the `std::move_only_function` has a
  target (public member function)
- **operator() (C++23)** — invokes the target (public member function)

### Non-member functions

- **swap(std::move_only_function) (C++23)** — overloads the `std::swap`
  algorithm (function)
- **operator== (C++23)** — compares a `std::move_only_function` with `nullptr`
  (function)

### Notes

Implementations may store a callable object of small size within the
`std::move_only_function` object. Such small object optimization is effectively
required for function pointers and `std::reference_wrapper` specializations, and
can only be applied to types `T` for which
`std::is_nothrow_move_constructible_v<T>` is `true`.

If a `std::move_only_function` returning a reference is initialized from a
function or function object returning a prvalue (including a lambda expression
without a trailing-return-type), the program is ill-formed because binding the
returned reference to a temporary object is forbidden. See also `std::function`
Notes.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_move_only_function` | 202110L | (C++23) | `std::move_only_function`

### Example

```cpp
#include <functional>
#include <future>
#include <iostream>

int main()
{
    std::packaged_task<double()> packaged_task([](){ return 3.14159; });

    std::future<double> future = packaged_task.get_future();

    auto lambda = [task = std::move(packaged_task)]() mutable { task(); };

//  std::function<void()> function = std::move(lambda); // Error
    std::move_only_function<void()> function = std::move(lambda); // OK

    function();

    std::cout << future.get();
}
```

Output:

```text
3.14159
```

### See also

- **function (C++11)** — wraps callable object of any copy constructible type
  with specified function call signature (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/move_only_function*
