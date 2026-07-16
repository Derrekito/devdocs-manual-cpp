# std::unsigned_integral

```cpp
template< class T >
concept unsigned_integral = std::integral<T> && !std::signed_integral<T>;  // (since C++20)
```

The concept `unsigned_integral<T>` is satisfied if and only if `T` is an
integral type and `std::is_signed_v<T>` is `false`.

### Notes

`unsigned_integral<T>` may be satisfied by a type that is not an unsigned
integer type, for example, `bool`.

### Example

```cpp
#include <concepts>
#include <iostream>
#include <string_view>

void test(std::signed_integral auto x, std::string_view text = "")
{
    std::cout << text << " (" + (text == "") << x << ") is a signed integral\n";
}

void test(std::unsigned_integral auto x, std::string_view text = "")
{
    std::cout << text << " (" + (text == "") << x << ") is an unsigned integral\n";
}

void test(auto x, std::string_view text = "")
{
    std::cout << text << " (" + (text == "") << x << ") is non-integral\n";
}

int main()
{
    test(42);               // signed
    test(0xFULL, "0xFULL"); // unsigned
    test('A');              // platform-dependent
    test(true, "true");     // unsigned
    test(4e-2, "4e-2");     // non-integral (hex-float)
    test("∫∫");             // non-integral
}
```

Possible output:

```text
(42) is a signed integral
0xFULL (15) is an unsigned integral
(A) is a signed integral
true (1) is an unsigned integral
4e-2 (0.04) is non-integral
(∫∫) is non-integral
```

### See also

- **is_integral (C++11)** — checks if a type is an integral type (class
  template)
- **is_signed (C++11)** — checks if a type is a signed arithmetic type (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/concepts/unsigned_integral*
