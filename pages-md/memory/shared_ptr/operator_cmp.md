# operator==, !=, <, <=, >, >=, <=> (std::shared_ptr)

```cpp
Compare two shared_ptr objects.
template< class T, class U >
bool operator==( const std::shared_ptr<T>& lhs,
                 const std::shared_ptr<U>& rhs ) noexcept;  // (1) (since C++11)
template< class T, class U >
bool operator!=( const std::shared_ptr<T>& lhs,
                 const std::shared_ptr<U>& rhs ) noexcept;  // (2) (since C++11) (until C++20)
template< class T, class U >
bool operator<( const std::shared_ptr<T>& lhs,
                const std::shared_ptr<U>& rhs ) noexcept;  // (3) (since C++11) (until C++20)
template< class T, class U >
bool operator>( const std::shared_ptr<T>& lhs,
                const std::shared_ptr<U>& rhs ) noexcept;  // (4) (since C++11) (until C++20)
template< class T, class U >
bool operator<=( const std::shared_ptr<T>& lhs,
                 const std::shared_ptr<U>& rhs ) noexcept;  // (5) (since C++11) (until C++20)
template< class T, class U >
bool operator>=( const std::shared_ptr<T>& lhs,
                 const std::shared_ptr<U>& rhs ) noexcept;  // (6) (since C++11) (until C++20)
template< class T, class U >
std::strong_ordering operator<=>( const std::shared_ptr<T>& lhs,
                                  const std::shared_ptr<U>& rhs ) noexcept;  // (7) (since C++20)
Compare a shared_ptr with a null pointer.
template< class T >
bool operator==( const std::shared_ptr<T>& lhs, std::nullptr_t ) noexcept;  // (8) (since C++11)
template< class T >
bool operator==( std::nullptr_t, const std::shared_ptr<T>& rhs ) noexcept;  // (9) (since C++11) (until C++20)
template< class T >
bool operator!=( const std::shared_ptr<T>& lhs, std::nullptr_t ) noexcept;  // (10) (since C++11) (until C++20)
template< class T >
bool operator!=( std::nullptr_t, const std::shared_ptr<T>& rhs ) noexcept;  // (11) (since C++11) (until C++20)
template< class T >
bool operator<( const std::shared_ptr<T>& lhs, std::nullptr_t ) noexcept;  // (12) (since C++11) (until C++20)
template< class T >
bool operator<( std::nullptr_t, const std::shared_ptr<T>& rhs ) noexcept;  // (13) (since C++11) (until C++20)
template< class T >
bool operator>( const std::shared_ptr<T>& lhs, std::nullptr_t ) noexcept;  // (14) (since C++11) (until C++20)
template< class T >
bool operator>( std::nullptr_t, const std::shared_ptr<T>& rhs ) noexcept;  // (15) (since C++11) (until C++20)
template< class T >
bool operator<=( const std::shared_ptr<T>& lhs, std::nullptr_t ) noexcept;  // (16) (since C++11) (until C++20)
template< class T >
bool operator<=( std::nullptr_t, const std::shared_ptr<T>& rhs ) noexcept;  // (17) (since C++11) (until C++20)
template< class T >
bool operator>=( const std::shared_ptr<T>& lhs, std::nullptr_t ) noexcept;  // (18) (since C++11) (until C++20)
template< class T >
bool operator>=( std::nullptr_t, const std::shared_ptr<T>& rhs ) noexcept;  // (19) (since C++11) (until C++20)
template< class T >
std::strong_ordering operator<=>( const std::shared_ptr<T>& lhs,
                                  std::nullptr_t ) noexcept;  // (20) (since C++20)
```

Compares two `shared_ptr<T>` objects or compares `shared_ptr<T>` with a null
pointer.

Note that the comparison operators for `shared_ptr` simply compare pointer
values; the actual objects pointed to are *not* compared. Having `operator<`
defined for `shared_ptr` allows `shared_ptr`s to be used as keys in associative
containers, like `std::map` and `std::set`.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.
*(since C++20)*

### Parameters

- **lhs** — the left-hand `shared_ptr` to compare
- **rhs** — the right-hand `shared_ptr` to compare

### Return value

1) `lhs.get() == rhs.get()`

2) `!(lhs == rhs)`

3) `std::less<V>()(lhs.get(), rhs.get())`, where V is the composite pointer type
   of `std::shared_ptr<T>::element_type*` and
   `std::shared_ptr<U>::element_type*`.

4) `rhs < lhs`

5) `!(rhs < lhs)`

6) `!(lhs < rhs)`

7) `std::compare_three_way{}(x.get(), y.get())`

8) `!lhs`

9) `!rhs`

10) `(bool)lhs`

11) `(bool)rhs`

12) `std::less<std::shared_ptr<T>::element_type*>()(lhs.get(), nullptr)`

13) `std::less<std::shared_ptr<T>::element_type*>()(nullptr, rhs.get())`

14) `nullptr < lhs`

15) `rhs < nullptr`

16) `!(nullptr < lhs)`

17) `!(rhs < nullptr)`

18) `!(lhs < nullptr)`

19) `!(nullptr < rhs)`

20) `std::compare_three_way{}(x.get(),
   static_cast<std::shared_ptr<T>::element_type*>(nullptr))`

### Notes

In all cases, it is the stored pointer (the one returned by `get()`) that is
compared, rather than the managed pointer (the one passed to the deleter when
`use_count` goes to zero). The two pointers may differ in a `shared_ptr` created
using the aliasing constructor.

### Example

```cpp
#include <iostream>
#include <memory>

int main()
{
    std::shared_ptr<int> p1(new int(42));
    std::shared_ptr<int> p2(new int(42));

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
  LWG 3427 | C++20 | `operator<=>(shared_ptr, nullptr_t)` was ill-formed |
      definition fixed

### See also

- **get** — returns the stored pointer (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/shared_ptr/operator_cmp*
