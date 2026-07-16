# std::array<T,N>::swap

```cpp
void swap( array& other ) noexcept(/* see below */);  // (since C++11) (constexpr since C++20)
```

Exchanges the contents of the container with those of `other`. Does not cause
iterators and references to associate with the other container.

### Parameters

- **other** — container to exchange the contents with

### Return value

(none)

### Exceptions

`noexcept` specification:
`noexcept(noexcept(swap(std::declval<T&>(), std::declval<T&>())))`
In the expression above, the identifier `swap` is looked up in the same manner
as the one used by the C++17 `std::is_nothrow_swappable` trait.
*(until C++17)*

`noexcept` specification:
`noexcept(std::is_nothrow_swappable_v<T>)`
*(since C++17)*

For zero-sized arrays,

`noexcept` specification:

`noexcept`

### Complexity

Linear in size of the container.

### Example

```cpp
#include <array>
#include <iostream>

template<class Os, class V> Os& operator<<(Os& os, const V& v)
{
    os << '{';
    for (auto i : v)
        os << ' ' << i;
    return os << " } ";
}

int main()
{
    std::array<int, 3> a1{1, 2, 3}, a2{4, 5, 6};

    auto it1 = a1.begin();
    auto it2 = a2.begin();
    int& ref1 = a1[1];
    int& ref2 = a2[1];

    std::cout << a1 << a2 << *it1 << ' ' << *it2 << ' ' << ref1 << ' ' << ref2 << '\n';
    a1.swap(a2);
    std::cout << a1 << a2 << *it1 << ' ' << *it2 << ' ' << ref1 << ' ' << ref2 << '\n';

    // Note that after swap iterators and references stay associated with their original
    // array, e.g. `it1` still points to element a1[0], `ref1` still refers to a1[1].
}
```

Output:

```text
{ 1 2 3 } { 4 5 6 } 1 4 2 5
{ 4 5 6 } { 1 2 3 } 4 1 5 2
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2456 | C++11 | the `noexcept` specification is ill-formed | made to work

### See also

- **std::swap(std::array) (C++11)** — specializes the `std::swap` algorithm
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/array/swap*
