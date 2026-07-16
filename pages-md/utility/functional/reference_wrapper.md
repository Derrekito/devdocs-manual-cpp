# std::reference_wrapper

```cpp
template< class T >
class reference_wrapper;  // (since C++11)
```

`std::reference_wrapper` is a class template that wraps a reference in a
copyable, assignable object. It is frequently used as a mechanism to store
references inside standard containers (like `std::vector`) which cannot normally
hold references.

Specifically, `std::reference_wrapper` is a CopyConstructible and CopyAssignable
wrapper around a reference to object or reference to function of type `T`.
Instances of `std::reference_wrapper` are objects (they can be copied or stored
in containers) but they are implicitly convertible to `T&`, so that they can be
used as arguments with the functions that take the underlying type by reference.

If the stored reference is Callable, `std::reference_wrapper` is callable with
the same arguments.

Helper functions `std::ref` and `std::cref` are often used to generate
`std::reference_wrapper` objects.

`std::reference_wrapper` is also used to pass objects by reference to
`std::bind`, the constructor of `std::thread`, or the helper functions
`std::make_pair` and `std::make_tuple`.

`std::reference_wrapper` is guaranteed to be TriviallyCopyable.
*(since C++17)*

`T` may be an incomplete type.
*(since C++20)*

### Member types

- **`type`** — `T`
- **`result_type` (deprecated in C++17)(removed in C++20)** — The return type of
  `T` if `T` is a function. Otherwise, not defined
- **`argument_type` (deprecated in C++17)(removed in C++20)** — if `T` is a
  function or pointer to function that takes one argument of type `A1`, then
  `argument_type` is `A1` if `T` is a pointer to member function of class `T0`
  that takes no arguments, then `argument_type` is `T0*`, possibly cv-qualified
  if `T` is a class type with a member type `T::argument_type`, then
  `argument_type` is an alias of that
- **`first_argument_type` (deprecated in C++17)(removed in C++20)** — if `T` is
  a function or pointer to function that takes two arguments of types `A1` and
  `A2`, then `first_argument_type` is `A1` if `T` is a pointer to member
  function of class `T0` that takes one argument, then `first_argument_type` is
  `T0*`, possibly cv-qualified if `T` is a class type with a member type
  `T::first_argument_type`, then `first_argument_type` is an alias of that
- **`second_argument_type` (deprecated in C++17)(removed in C++20)** — if `T` is
  a function or pointer to function that takes two arguments of type s `A1` and
  `A2`, then `second_argument_type` is `A2` if `T` is a pointer to member
  function of class `T0` that takes one argument `A1`, then
  `second_argument_type` is `A1`, possibly cv-qualified if `T` is a class type
  with a member type `T::second_argument_type`, then `second_argument_type` is
  an alias of that

### Member functions

- **(constructor)** — stores a reference in a new `std::reference_wrapper`
  object (public member function)
- **operator=** — rebinds a `std::reference_wrapper` (public member function)
- **getoperator T&** — accesses the stored reference (public member function)
- **operator()** — calls the stored function (public member function)

### Deduction guides(since C++17)

### Helper classes

- **std::basic_common_reference<std::reference_wrapper> (C++23)** — determines
  the common reference type of `reference_wrapper` and non-`reference_wrapper`
  (class template specialization)

### Possible implementation

```cpp
namespace detail
{
    template<class T> constexpr T& FUN(T& t) noexcept { return t; }
    template<class T> void FUN(T&&) = delete;
}

template<class T>
class reference_wrapper
{
public:
    // types
    using type = T;

    // construct/copy/destroy
    template<class U, class = decltype(
        detail::FUN<T>(std::declval<U>()),
        std::enable_if_t<!std::is_same_v<reference_wrapper, std::remove_cvref_t<U>>>()
    )>
    constexpr reference_wrapper(U&& u)
        noexcept(noexcept(detail::FUN<T>(std::forward<U>(u))))
        : _ptr(std::addressof(detail::FUN<T>(std::forward<U>(u)))) {}

    reference_wrapper(const reference_wrapper&) noexcept = default;

    // assignment
    reference_wrapper& operator=(const reference_wrapper& x) noexcept = default;

    // access
    constexpr operator T& () const noexcept { return *_ptr; }
    constexpr T& get() const noexcept { return *_ptr; }

    template<class... ArgTypes>
    constexpr std::invoke_result_t<T&, ArgTypes...>
        operator() (ArgTypes&&... args ) const
            noexcept(std::is_nothrow_invocable_v<T&, ArgTypes...>)
    {
        return std::invoke(get(), std::forward<ArgTypes>(args)...);
    }

private:
    T* _ptr;
};

// deduction guides
template<class T>
reference_wrapper(T&) -> reference_wrapper<T>;
```

### Example

Demonstrates the use of `std::reference_wrapper` as a container of references,
which makes it possible to access the same container using multiple indexes.

```cpp
#include <algorithm>
#include <functional>
#include <iostream>
#include <list>
#include <numeric>
#include <random>
#include <vector>

void print(auto const rem, std::ranges::range auto const& v)
{
    for (std::cout << rem; auto const& e : v)
        std::cout << e << ' ';
    std::cout << '\n';
}

int main()
{
    std::list<int> l(10);
    std::iota(l.begin(), l.end(), -4);

    // can't use shuffle on a list (requires random access), but can use it on a vector
    std::vector<std::reference_wrapper<int>> v(l.begin(), l.end());

    std::ranges::shuffle(v, std::mt19937{std::random_device{}()});

    print("Contents of the list: ", l);
    print("Contents of the list, as seen through a shuffled vector: ", v);

    std::cout << "Doubling the values in the initial list...\n";
    std::ranges::for_each(l, [](int& i) { i *= 2; });

    print("Contents of the list, as seen through a shuffled vector: ", v);
}
```

Possible output:

```text
Contents of the list: -4 -3 -2 -1 0 1 2 3 4 5
Contents of the list, as seen through a shuffled vector: -1 2 -2 1 5 0 3 -3 -4 4
Doubling the values in the initial list...
Contents of the list, as seen through a shuffled vector: -2 4 -4 2 10 0 6 -6 -8 8
```

### See also

- **refcref (C++11)(C++11)** — creates a `std::reference_wrapper` with a type
  deduced from its argument (function template)
- **bind (C++11)** — binds one or more arguments to a function object (function
  template)
- **unwrap_referenceunwrap_ref_decay (C++20)(C++20)** — get the reference type
  wrapped in `std::reference_wrapper` (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/reference_wrapper*
