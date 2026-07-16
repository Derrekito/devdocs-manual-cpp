# operator==,!=,<,<=,>,>=,<=>(std::unique_ptr)

```cpp
template< class T1, class D1, class T2, class D2 >
bool operator==( const unique_ptr<T1, D1>& x, const unique_ptr<T2, D2>& y );  // (1) (since C++11) (constexpr since C++23)
template< class T1, class D1, class T2, class D2 >
bool operator!=( const unique_ptr<T1, D1>& x, const unique_ptr<T2, D2>& y );  // (2) (since C++11) (until C++20)
template< class T1, class D1, class T2, class D2 >
bool operator<( const unique_ptr<T1, D1>& x, const unique_ptr<T2, D2>& y );  // (3) (since C++11)
template< class T1, class D1, class T2, class D2 >
bool operator<=( const unique_ptr<T1, D1>& x, const unique_ptr<T2, D2>& y );  // (4) (since C++11)
template< class T1, class D1, class T2, class D2 >
bool operator>( const unique_ptr<T1, D1>& x, const unique_ptr<T2, D2>& y );  // (5) (since C++11)
template< class T1, class D1, class T2, class D2 >
bool operator>=( const unique_ptr<T1, D1>& x, const unique_ptr<T2, D2>& y );  // (6) (since C++11)
template< class T1, class D1, class T2, class D2 >
    requires std::three_way_comparable_with<
        typename unique_ptr<T1, D1>::pointer,
        typename unique_ptr<T2, D2>::pointer>
std::compare_three_way_result_t<typename unique_ptr<T1, D1>::pointer,
                                typename unique_ptr<T2, D2>::pointer>
    operator<=>( const unique_ptr<T1, D1>& x, const unique_ptr<T2, D2>& y );  // (7) (since C++20)
template< class T, class D >
bool operator==( const unique_ptr<T, D>& x, std::nullptr_t ) noexcept;  // (8) (since C++11) (constexpr since C++23)
template< class T, class D >
bool operator==( std::nullptr_t, const unique_ptr<T, D>& x ) noexcept;  // (9) (since C++11) (until C++20)
template< class T, class D >
bool operator!=( const unique_ptr<T, D>& x, std::nullptr_t ) noexcept;  // (10) (since C++11) (until C++20)
template< class T, class D >
bool operator!=( std::nullptr_t, const unique_ptr<T, D>& x ) noexcept;  // (11) (since C++11) (until C++20)
template< class T, class D >
bool operator<( const unique_ptr<T, D>& x, std::nullptr_t );  // (12) (since C++11) (constexpr since C++23)
template< class T, class D >
bool operator<( std::nullptr_t, const unique_ptr<T, D>& y );  // (13) (since C++11) (constexpr since C++23)
template< class T, class D >
bool operator<=( const unique_ptr<T, D>& x, std::nullptr_t );  // (14) (since C++11) (constexpr since C++23)
template< class T, class D >
bool operator<=( std::nullptr_t, const unique_ptr<T, D>& y );  // (15) (since C++11) (constexpr since C++23)
template< class T, class D >
bool operator>( const unique_ptr<T, D>& x, std::nullptr_t );  // (16) (since C++11) (constexpr since C++23)
template< class T, class D >
bool operator>( std::nullptr_t, const unique_ptr<T, D>& y );  // (17) (since C++11) (constexpr since C++23)
template< class T, class D >
bool operator>=( const unique_ptr<T, D>& x, std::nullptr_t );  // (18) (since C++11) (constexpr since C++23)
template< class T, class D >
bool operator>=( std::nullptr_t, const unique_ptr<T, D>& y );  // (19) (since C++11) (constexpr since C++23)
template< class T, class D >
    requires std::three_way_comparable<typename unique_ptr<T, D>::pointer>
std::compare_three_way_result_t<typename unique_ptr<T, D>::pointer>
    operator<=>( const unique_ptr<T, D>& x, std::nullptr_t );  // (20) (since C++20) (constexpr since C++23)
```

Compares the pointer values of two `unique_ptr`s, or a `unique_ptr` and
`nullptr`.

1-7) Compares two `unique_ptr`s.

8-20) Compares a `unique_ptr` and `nullptr`.

The `!=` operator is synthesized from `operator==`.
*(since C++20)*

### Parameters

- **x, y** — `unique_ptr`s to compare

### Return value

1) `x.get() == y.get()`

2) `x.get() != y.get()`

3) `std::less<CT>()(x.get(), y.get())`, where `CT` is
   `std::common_type<unique_ptr<T1, D1>::pointer, unique_ptr<T2,
   D2>::pointer>::type`.

4) `!(y < x)`

5) `y < x`

6) `!(x < y)`

7) `std::compare_three_way{}(x.get(), y.get())`

8,9) `!x`

10,11) `(bool)x`

12) `std::less<unique_ptr<T,D>::pointer>()(x.get(), nullptr)`

13) `std::less<unique_ptr<T,D>::pointer>()(nullptr, y.get())`

14) `!(nullptr < x)`

15) `!(y < nullptr)`

16) `nullptr < x`

17) `y < nullptr`

18) `!(x < nullptr)`

19) `!(nullptr < y)`

20) `std::compare_three_way{}(x.get(), static_cast<typename unique_ptr<T,
   D>::pointer>(nullptr))`

### Example

```cpp
#include <iostream>
#include <memory>

int main()
{
    std::unique_ptr<int> p1(new int(42));
    std::unique_ptr<int> p2(new int(42));

    std::cout << std::boolalpha
        << "(p1 == p1)       : " << (p1 == p1) << '\n'
        << "(p1 <=> p1) == 0 : " << ((p1 <=> p1) == 0) << '\n' // Since C++20

    // p1 and p2 point to different memory locations, so p1 != p2
        << "(p1 == p2)       : " << (p1 == p2) << '\n'
        << "(p1 < p2)        : " << (p1 < p2) << '\n'
        << "(p1 <=> p2) < 0  : " << ((p1 <=> p2) < 0) << '\n'   // Since C++20
        << "(p1 <=> p2) == 0 : " << ((p1 <=> p2) == 0) << '\n'; // Since C++20
}
```

Possible output:

```text
(p1 == p1)       : true
(p1 <=> p1) == 0 : true
(p1 == p2)       : false
(p1 < p2)        : true
(p1 <=> p2) < 0  : true
(p1 <=> p2) == 0 : false
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3426 | C++20 | `operator<=>(unique_ptr, nullptr_t)` was ill-formed |
      constraints and definition fixed

### See also

- **get** — returns a pointer to the managed object (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/unique_ptr/operator_cmp*
