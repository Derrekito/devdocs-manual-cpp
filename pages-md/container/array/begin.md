# std::array<T,N>::begin, std::array<T,N>::cbegin

```cpp
iterator begin() noexcept;  // (1) (since C++11) (constexpr since C++17)
const_iterator begin() const noexcept;  // (2) (since C++11) (constexpr since C++17)
const_iterator cbegin() const noexcept;  // (3) (since C++11) (constexpr since C++17)
```

Returns an iterator to the first element of the `array`.

If the `array` is empty, the returned iterator will be equal to `end()`.

### Parameters

(none)

### Return value

Iterator to the first element.

### Complexity

Constant.

### Example

```cpp
#include <algorithm>
#include <array>
#include <iomanip>
#include <iostream>

int main()
{
    std::cout << std::boolalpha;

    std::array<int, 0> empty;
    std::cout << "1) "
              << (empty.begin() == empty.end()) << ' '     // true
              << (empty.cbegin() == empty.cend()) << '\n'; // true
    // *(empty.begin()) = 42; // => undefined behavior at run-time

    std::array<int, 4> numbers{5, 2, 3, 4};
    std::cout << "2) "
              << (numbers.begin() == numbers.end()) << ' '    // false
              << (numbers.cbegin() == numbers.cend()) << '\n' // false
              << "3) "
              << *(numbers.begin()) << ' '    // 5
              << *(numbers.cbegin()) << '\n'; // 5

    *numbers.begin() = 1;
    std::cout << "4) " << *(numbers.begin()) << '\n'; // 1
    // *(numbers.cbegin()) = 42; // compile-time error:
                                 // read-only variable is not assignable

    // print out all elements
    std::cout << "5) ";
    std::for_each(numbers.cbegin(), numbers.cend(), [](int x)
    {
        std::cout << x << ' ';
    });
    std::cout << '\n';

    constexpr std::array constants{'A', 'B', 'C'};
    static_assert(constants.begin() != constants.end());   // OK
    static_assert(constants.cbegin() != constants.cend()); // OK
    static_assert(*constants.begin() == 'A');              // OK
    static_assert(*constants.cbegin() == 'A');             // OK
    // *constants.begin() = 'Z'; // compile-time error:
                                 // read-only variable is not assignable
}
```

Output:

```text
1) true true
2) false false
3) 5 5
4) 1
5) 1 2 3 4
```

### See also

- **endcend** — returns an iterator to the end (public member function)
- **begincbegin (C++11)(C++14)** — returns an iterator to the beginning of a
  container or array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/array/begin*
