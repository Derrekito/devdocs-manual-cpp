# std::variant<Types...>::operator=

```cpp
constexpr variant& operator=( const variant& rhs );  // (1) (since C++17)
constexpr variant& operator=( variant&& rhs ) noexcept(/* see below */);  // (2) (since C++17)
template< class T >
variant& operator=( T&& t ) noexcept(/* see below */);  // (since C++17) (until C++20)
template< class T >
constexpr variant& operator=( T&& t ) noexcept(/* see below */);  // (since C++20)
```

Assigns a new value to an existing `variant` object.

1) Copy-assignment:

- If both `*this` and `rhs` are valueless by exception, does nothing.
- Otherwise, if `rhs` is valueless, but `*this` is not, destroys the value
  contained in `*this` and makes it valueless.
- Otherwise, if `rhs` holds the same alternative as `*this`, assigns the value
  contained in `rhs` to the value contained in `*this`. If an exception is
  thrown, `*this` does not become valueless: the value depends on the exception
  safety guarantee of the alternative's copy assignment.
- Otherwise, if the alternative held by `rhs` is either nothrow copy
  constructible or *not* nothrow move constructible (as determined by
  `std::is_nothrow_copy_constructible` and `std::is_nothrow_move_constructible`,
  respectively), equivalent to
  `this->emplace<rhs.index()>(*std::get_if<rhs.index()>(std::addressof(rhs)))`.
  `*this` may become `valueless_by_exception` if an exception is thrown on the
  copy-construction inside `emplace`.
- Otherwise, equivalent to `this->operator=(variant(rhs))`.

This overload is defined as deleted unless `std::is_copy_constructible_v<T_i>`
   and `std::is_copy_assignable_v<T_i>` are both `true` for all `T_i` in
   `Types...`. This overload is trivial if
   `std::is_trivially_copy_constructible_v<T_i>`,`std::is_trivially_copy_assignable_v<T_i>`
   and `std::is_trivially_destructible_v<T_i>` are all `true` for all `T_i` in
   `Types...`.
2) Move-assignment:

- If both `*this` and `rhs` are valueless by exception, does nothing.
- Otherwise, if `rhs` is valueless, but `*this` is not, destroys the value
  contained in `*this` and makes it valueless.
- Otherwise, if `rhs` holds the same alternative as `*this`, assigns
  `std::move(*std::get_if<j>(std::addressof(rhs)))` to the value contained in
  `*this`, with `j` being `index()`. If an exception is thrown, `*this` does not
  become valueless: the value depends on the exception safety guarantee of the
  alternative's move assignment.
- Otherwise (if `rhs` and `*this` hold different alternatives), equivalent to
  `this->emplace<rhs.index()>(std::move(*std::get_if<rhs.index()>(std::addressof(rhs))))`.
  If an exception is thrown by `T_i`'s move constructor, `*this` becomes
  `valueless_by_exception`.

This overload participates in overload resolution only if
   `std::is_move_constructible_v<T_i>` and `std::is_move_assignable_v<T_i>` are
   both `true` for all `T_i` in `Types...`. This overload is trivial if
   `std::is_trivially_move_constructible_v<T_i>`,
   `std::is_trivially_move_assignable_v<T_i>`, and
   `std::is_trivially_destructible_v<T_i>` are all `true` for all `T_i` in
   `Types...`.
3) Converting assignment.

- Determines the alternative type `T_j` that would be selected by overload
  resolution for the expression `F(std::forward<T>(t))` if there was an overload
  of imaginary function `F(T_i)` for every `T_i` from `Types...` in scope at the
  same time, except that:
- If `*this` already holds a `T_j`, assigns `std::forward<T>(t)` to the value
  contained in `*this`. If an exception is thrown, `*this` does not become
  valueless: the value depends on the exception safety guarantee of the
  assignment called.
- Otherwise, if `std::is_nothrow_constructible_v<T_j, T> ||
  !std::is_nothrow_move_constructible_v<T_j>` is `true`, equivalent to
  `this->emplace<j>(std::forward<T>(t))`. `*this` may become
  `valueless_by_exception` if an exception is thrown on the initialization
  inside `emplace`.
- Otherwise, equivalent to `this->emplace<j>(T_j(std::forward<T>(t)))`.

This overload participates in overload resolution only if
`std::decay_t<T>`(until C++20)`std::remove_cvref_t<T>`(since C++20) is not the
same type as `variant` and `std::is_assignable_v<T_j&, T>` is `true` and
`std::is_constructible_v<T_j, T>` is `true` and the expression
`F(std::forward<T>(t))` (with F being the above-mentioned set of imaginary
functions) is well formed.

```cpp
std::variant<std::string> v1;
v1 = "abc"; // OK
std::variant<std::string, std::string> v2;
v2 = "abc"; // Error
std::variant <std::string, bool> v3;
v3 = "abc"; // OK, chooses string; bool is not a candidate
std::variant<float, long, double> v4; //holds float
v4 = 0; // OK, holds long; float and double are not candidates
```

### Parameters

- **rhs** — another `variant`
- **t** — a value convertible to one of the variant's alternatives

### Return value

`*this`

### Exceptions

1) May throw any exception thrown by assignment and copy/move initialization of
   any alternative.

2) `noexcept` specification:
   `noexcept(((std::is_nothrow_move_constructible_v<Types> &&
   std::is_nothrow_move_assignable_v<Types>) && ...))`

3) `noexcept` specification: `noexcept(std::is_nothrow_assignable_v<T_j&, T> &&
   std::is_nothrow_constructible_v<T_j, T>)`

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <string>
#include <type_traits>
#include <variant>

std::ostream& operator<<(std::ostream& os, std::variant<int, std::string> const& va)
{
    os << ": { ";

    std::visit([&](auto&& arg)
    {
        using T = std::decay_t<decltype(arg)>;
        if constexpr (std::is_same_v<T, int>)
            os << arg;
        else if constexpr (std::is_same_v<T, std::string>)
            os << std::quoted(arg);
    }, va);

    return os << " };\n";
}

int main()
{
    std::variant<int, std::string> a{2017}, b{"CppCon"};
    std::cout << "a" << a << "b" << b << '\n';

    std::cout << "(1) operator=( const variant& rhs )\n";
    a = b;
    std::cout << "a" << a << "b" << b << '\n';

    std::cout << "(2) operator=( variant&& rhs )\n";
    a = std::move(b);
    std::cout << "a" << a << "b" << b << '\n';

    std::cout << "(3) operator=( T&& t ), where T is int\n";
    a = 2019;
    std::cout << "a" << a << '\n';

    std::cout << "(3) operator=( T&& t ), where T is std::string\n";
    std::string s{"CppNow"};
    std::cout << "s: " << std::quoted(s) << '\n';
    a = std::move(s);
    std::cout << "a" << a << "s: " << std::quoted(s) << '\n';
}
```

Possible output:

```text
a: { 2017 };
b: { "CppCon" };

(1) operator=( const variant& rhs )
a: { "CppCon" };
b: { "CppCon" };

(2) operator=( variant&& rhs )
a: { "CppCon" };
b: { "" };

(3) operator=( T&& t ), where T is int
a: { 2019 };

(3) operator=( T&& t ), where T is std::string
s: "CppNow"
a: { "CppNow" };
s: ""
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3024 | C++17 | copy assignment operator doesn't participate in overload
      resolution if any member type is not copyable | defined as deleted instead
  P0602R4 | C++17 | copy/move assignment may not be trivial even if underlying
      operations are trivial | required to propagate triviality
  P0608R3 | C++17 | converting assignment blindly assembles an overload set,
      leading to unintended conversions | narrowing and boolean conversions not
      considered
  P2231R1 | C++20 | converting assignment was not constexpr while the required
      operations can be in C++20 | made constexpr
  LWG 3585 | C++17 | converting assignment was sometimes unexpectedly ill-formed
      because there was no available move assignment | made well-formed

### See also

- **emplace** — constructs a value in the variant, in place (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/utility/variant/operator%3D*
