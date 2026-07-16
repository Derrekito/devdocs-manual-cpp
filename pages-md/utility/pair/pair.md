# std::pair<T1,T2>::pair

```cpp
pair();  // (until C++11)
constexpr pair();  // (since C++11) (conditionally explicit)
pair( const T1& x, const T2& y );  // (until C++11)
pair( const T1& x, const T2& y );  // (since C++11) (until C++14) (conditionally explicit)
constexpr pair( const T1& x, const T2& y );  // (since C++14) (conditionally explicit)
template< class U1, class U2 >
pair( U1&& x, U2&& y );  // (since C++11) (until C++14) (conditionally explicit)
template< class U1, class U2 >
constexpr pair( U1&& x, U2&& y );  // (since C++14) (until C++23) (conditionally explicit)
template< class U1 = T1, class U2 = T2 >
constexpr pair( U1&& x, U2&& y );  // (since C++23) (conditionally explicit)
template< class U1, class U2 >
constexpr pair( pair<U1, U2>& p );  // (4) (since C++23) (conditionally explicit)
template< class U1, class U2 >
pair( const pair<U1, U2>& p );  // (until C++11)
template< class U1, class U2 >
pair( const pair<U1, U2>& p );  // (since C++11) (until C++14) (conditionally explicit)
template< class U1, class U2 >
constexpr pair( const pair<U1, U2>& p );  // (since C++14) (conditionally explicit)
template< class U1, class U2 >
pair( pair<U1, U2>&& p );  // (since C++11) (until C++14) (conditionally explicit)
template< class U1, class U2 >
constexpr pair( pair<U1, U2>&& p );  // (since C++14) (conditionally explicit)
template< class U1, class U2 >
constexpr pair( const pair<U1, U2>&& p );  // (7) (since C++23) (conditionally explicit)
template< pair-like P >
constexpr pair ( P&& u );  // (8) (since C++23) (conditionally explicit)
template< class... Args1, class... Args2 >
pair( std::piecewise_construct_t,
      std::tuple<Args1...> first_args,
      std::tuple<Args2...> second_args );  // (since C++11) (until C++20)
template< class... Args1, class... Args2 >
constexpr pair( std::piecewise_construct_t,
                std::tuple<Args1...> first_args,
                std::tuple<Args2...> second_args );  // (since C++20)
pair( const pair& p ) = default;  // (10)
pair( pair&& p ) = default;  // (11) (since C++11)
```

Constructs a new pair.

1) Default constructor. Value-initializes both elements of the pair, `first` and
   `second`. This constructor participates in overload resolution if and only if
   `std::is_default_constructible_v<T1>` and
   `std::is_default_constructible_v<T2>` are both `true`. This constructor is
   explicit if and only if either `T1` or `T2` is not implicitly
   default-constructible. (since C++11)

2) Initializes `first` with `x` and `second` with `y`. This constructor
   participates in overload resolution if and only if
   `std::is_copy_constructible_v<T1>` and `std::is_copy_constructible_v<T2>` are
   both `true`. This constructor is explicit if and only if
   `std::is_convertible_v<const T1&, T1>` is `false` or
   `std::is_convertible_v<const T2&, T2>` is `false`. (since C++11)

3) Initializes `first` with `std::forward<U1>(x)` and `second` with
   `std::forward<U2>(y)`.

This constructor participates in overload resolution if and only if
   `std::is_constructible_v<T1, U1>` and `std::is_constructible_v<T2, U2>` are
   both `true`.

This constructor is `explicit` if and only if `std::is_convertible_v<U1, T1>` is
   `false` or `std::is_convertible_v<U2, T2>` is `false`. This constructor is
   defined as deleted if the initialization of `first` or `second` would bind a
   reference to temporary object. (since C++23)

4) Initializes `first` with `p.first` and `second` with `p.second`.

This constructor participates in overload resolution if and only if
   `std::is_constructible_v<T1, U1&>` and `std::is_constructible_v<T2, U2&>` are
   both `true`.

This constructor is `explicit` if and only if `std::is_convertible_v<U1&, T1>`
   is `false` or `std::is_convertible_v<U2&, T2>` is `false`.

This constructor is defined as deleted if the initialization of `first` or
   `second` would bind a reference to temporary object.

5) Initializes `first` with `p.first` and `second` with `p.second`. This
   constructor participates in overload resolution if and only if
   `std::is_constructible_v<T1, const U1&>` and `std::is_constructible_v<T2,
   const U2&>` are both `true`. This constructor is explicit if and only if
   `std::is_convertible_v<const U1&, T1>` is `false` or
   `std::is_convertible_v<const U2&, T2>` is `false`. (since C++11) This
   constructor is defined as deleted if the initialization of `first` or
   `second` would bind a reference to temporary object. (since C++23)

6) Initializes `first` with `std::forward<U1>(p.first)` and `second` with
   `std::forward<U2>(p.second)`.

This constructor participates in overload resolution if and only if
   `std::is_constructible_v<T1, U1>` and `std::is_constructible_v<T2, U2>` are
   both `true`.

This constructor is `explicit` if and only if `std::is_convertible_v<U1, T1>` is
   `false` or `std::is_convertible_v<U2, T2>` is `false`. This constructor is
   defined as deleted if the initialization of `first` or `second` would bind a
   reference to temporary object. (since C++23)

7) Initializes `first` with `std::forward<const U1>(p.first)` and `second` with
   `std::forward<const U2>(p.second)`.

This constructor participates in overload resolution if and only if
   `std::is_constructible_v<T1, U1>` and `std::is_constructible_v<T2, U2>` are
   both `true`.

This constructor is `explicit` if and only if `std::is_convertible_v<const U1,
   T1>` is `false` or `std::is_convertible_v<const U2, T2>` is `false`.

This constructor is defined as deleted if the initialization of `first` or
   `second` would bind a reference to temporary object.

8) Given `u1` as `std::get<0>(std::forward(u))` and `u2` as
   `std::get<1>(std::forward(u))`, denote their types as `U1` and `U2`
   respectively. Initializes `first` with `u1` and `second` with `u2`.
This constructor participates in overload resolution if and only if

- std::remove_cvref(P) is not a specialization of `std::ranges::subrange`,
- `std::is_constructible_v<T1, U1>` is `true`, and
- `std::is_constructible_v<T2, U2` is `true`.

This constructor is `explicit` if and only if `std::is_convertible_v<U1, T1>` is
   `false` or `std::is_convertible_v<U2, T2>` is `false`.

This constructor is defined as deleted if the initialization of `first` or
   `second` would bind a reference to temporary object.

9) Forwards the elements of `first_args` to the constructor of `first` and
   forwards the elements of `second_args` to the constructor of `second`. This
   is the only non-default constructor that can be used to create a pair of
   non-copyable non-movable types. The program is ill-formed if `first` or
   `second` is a reference and bound to a temporary object.

10) Copy constructor is implicitly declared(until C++11)defaulted, and is
   `constexpr` if copying of both elements satisfies the requirements on
   constexpr functions(since C++11).

11) Move constructor is defaulted, and is `constexpr` if moving of both elements
   satisfies the requirements on constexpr functions.

### Parameters

- **x** — value to initialize the first element of this pair
- **y** — value to initialize the second element of this pair
- **p** — pair of values used to initialize both elements of this pair
- **u** — `pair-like` object of values used to initialize both elements of this
  pair
- **first_args** — tuple of constructor arguments to initialize the first
  element of this pair
- **second_args** — tuple of constructor arguments to initialize the second
  element of this pair

### Exceptions

Does not throw exceptions unless one of the specified operations (e.g.
constructor of an element) throws.

### Example

```cpp
#include <complex>
#include <iostream>
#include <string>
#include <tuple>
#include <utility>

int main()
{
    auto print = [](auto rem, auto const& pair)
    {
        std::cout << rem << "(" << pair.first << ", " << pair.second << ")\n";
    };

    std::pair<int, float> p1;
    print("(1) Value-initialized: ", p1);

    std::pair<int, double> p2{42, 3.1415};
    print("(2) Initialized with two values: ", p2);

    std::pair<char, int> p4{p2};
    print("(4) Implicitly converted: ", p4);

    std::pair<std::complex<double>, std::string> p6
        {std::piecewise_construct, std::forward_as_tuple(0.123, 7.7),
            std::forward_as_tuple(10, 'a')};
    print("(8) Piecewise constructed: ", p6);
}
```

Possible output:

```text
(1) Value-initialized: (0, 0)
(2) Initialized with two values: (42, 3.1415)
(4) Implicitly converted: (*, 3)
(8) Piecewise constructed: ((0.123,7.7), aaaaaaaaaa)
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 265 | C++98 | the default constructor copy-initialized `first` and
      `second` with `T1()` and `T2()` respectively (thus required `T1` and `T2`
      to be CopyConstructible) | `first` and `second` are value-initialized
  LWG 2510 | C++11 | the default constructor was implicit | made
      conditionally-explicit
  N4387 | C++11 | some constructors were implicit-only, preventing some uses |
      constructors made conditionally-explicit

### See also

- **make_pair** — creates a `pair` object of type, defined by the argument types
  (function template)
- **(constructor)** — constructs a new `tuple` (public member function of
  `std::tuple<Types...>`)

---
*Source: https://en.cppreference.com/w/cpp/utility/pair/pair*
