# std::ref, std::cref

```cpp
template< class T >
std::reference_wrapper<T> ref( T& t ) noexcept;  // (since C++11) (until C++20)
template< class T >
constexpr std::reference_wrapper<T> ref( T& t ) noexcept;  // (since C++20)
template< class T >
std::reference_wrapper<T>
    ref( std::reference_wrapper<T> t ) noexcept;  // (since C++11) (until C++20)
template< class T >
constexpr std::reference_wrapper<T>
    ref( std::reference_wrapper<T> t ) noexcept;  // (since C++20)
template< class T >
void ref( const T&& ) = delete;  // (3) (since C++11)
template< class T >
std::reference_wrapper<const T> cref( const T& t ) noexcept;  // (since C++11) (until C++20)
template< class T >
constexpr std::reference_wrapper<const T> cref( const T& t ) noexcept;  // (since C++20)
template< class T >
std::reference_wrapper<const T>
    cref( std::reference_wrapper<T> t ) noexcept;  // (since C++11) (until C++20)
template< class T >
constexpr std::reference_wrapper<const T>
    cref( std::reference_wrapper<T> t ) noexcept;  // (since C++20)
template< class T >
void cref( const T&& ) = delete;  // (6) (since C++11)
```

Function templates `ref` and `cref` are helper functions that generate an object
of type `std::reference_wrapper`, using template argument deduction to determine
the template argument of the result.

`T` may be an incomplete type.
*(since C++20)*

### Parameters

- **t** — lvalue reference to object that needs to be wrapped or an instance of
  `std::reference_wrapper`

### Return value

1) `std::reference_wrapper<T>(t)`

2) `t`

4) `std::reference_wrapper<const T>(t)`

5) `t`

3,6) rvalue reference wrapper is deleted.

### Example

```cpp
#include <functional>
#include <iostream>

void f(int& n1, int& n2, const int& n3)
{
    std::cout << "In function: " << n1 << ' ' << n2 << ' ' << n3 << '\n';
    ++n1; // increments the copy of n1 stored in the function object
    ++n2; // increments the main()'s n2
    // ++n3; // compile error
}

int main()
{
    int n1 = 1, n2 = 2, n3 = 3;
    std::function<void()> bound_f = std::bind(f, n1, std::ref(n2), std::cref(n3));
    n1 = 10;
    n2 = 11;
    n3 = 12;
    std::cout << "Before function: " << n1 << ' ' << n2 << ' ' << n3 << '\n';
    bound_f();
    std::cout << "After function: " << n1 << ' ' << n2 << ' ' << n3 << '\n';
}
```

Output:

```text
Before function: 10 11 12
In function: 1 11 12
After function: 10 12 12
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3146 | C++11 | unwrapping overloads sometimes led to error | made always
      valid

### See also

- **reference_wrapper (C++11)** — CopyConstructible and CopyAssignable reference
  wrapper (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/ref*
