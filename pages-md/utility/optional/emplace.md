# std::optional<T>::emplace

```cpp
template< class... Args >
T& emplace( Args&&... args );  // (since C++17) (until C++20)
template< class... Args >
constexpr T& emplace( Args&&... args );  // (since C++20)
template< class U, class... Args >
T& emplace( std::initializer_list<U> ilist, Args&&... args );  // (since C++17) (until C++20)
template< class U, class... Args >
constexpr T& emplace( std::initializer_list<U> ilist, Args&&... args );  // (since C++20)
```

Constructs the contained value in-place. If `*this` already contains a value
before the call, the contained value is destroyed by calling its destructor.

1) Initializes the contained value by direct-initializing (but not
   direct-list-initializing) with `std::forward<Args>(args)...` as parameters.

2) Initializes the contained value by calling its constructor with `ilist,
   std::forward<Args>(args)...` as parameters. This overload participates in
   overload resolution only if `std::is_constructible<T,
   std::initializer_list<U>&, Args&&...>::value` is `true`.

### Parameters

- **args...** — the arguments to pass to the constructor
- **ilist** — the initializer list to pass to the constructor

**Type requirements**

**-`T` must be constructible from `Args...` for overload (1)**

**-`T` must be constructible from `std::initializer_list` and `Args...` for overload (2)**

### Return value

A reference to the new contained value.

### Exceptions

Any exception thrown by the selected constructor of `T`. If an exception is
thrown, `*this` does not contain a value after this call (the previously
contained value, if any, had been destroyed).

### Example

```cpp
#include <optional>
#include <iostream>

struct A {
    std::string s;
    A(std::string str) : s(std::move(str)), id{n++} { note("+ constructed"); }
    ~A() { note("~ destructed"); }
    A(const A& o) : s(o.s), id{n++} { note("+ copy constructed"); }
    A(A&& o) : s(std::move(o.s)), id{n++} { note("+ move constructed"); }
    A& operator=(const A& other) {
        s = other.s;
        note("= copy assigned");
        return *this;
    }
    A& operator=(A&& other) {
        s = std::move(other.s);
        note("= move assigned");
        return *this;
    }
    inline static int n{};
    int id{};
    void note(auto s) { std::cout << "  " << s << " #" << id << '\n'; }
};

int main()
{
    std::optional<A> opt;

    std::cout << "Assign:\n";
    opt = A("Lorem ipsum dolor sit amet, consectetur adipiscing elit nec.");

    std::cout << "Emplace:\n";
    // As opt contains a value it will also destroy that value
    opt.emplace("Lorem ipsum dolor sit amet, consectetur efficitur.");

    std::cout << "End example\n";
}
```

Output:

```text
Assign:
  + constructed #0
  + move constructed #1
  ~ destructed #0
Emplace:
  ~ destructed #1
  + constructed #2
End example
  ~ destructed #2
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2231R1 | C++20 | `emplace` was not constexpr while the required operations
      can be constexpr in C++20 | made constexpr

### See also

- **operator=** — assigns contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/optional/emplace*
