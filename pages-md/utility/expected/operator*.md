# std::expected<T,E>::operator->, std::expected<T,E>::operator*

```cpp
T is not cv void
constexpr const T* operator->() const noexcept;  // (1) (since C++23)
constexpr T* operator->() noexcept;  // (1) (since C++23)
constexpr const T& operator*() const& noexcept;  // (2) (since C++23)
constexpr T& operator*() & noexcept;  // (2) (since C++23)
constexpr const T&& operator*() const&& noexcept;  // (2) (since C++23)
constexpr T&& operator*() && noexcept;  // (2) (since C++23)
T is cv void
constexpr void operator*() const noexcept;  // (3) (since C++23)
```

Accesses the expected value contained in `*this`.

1) Returns a pointer to the contained value.

2) Returns a reference to the contained value.

3) Returns nothing.

The behavior is undefined if `this->has_value()` is false.

### Parameters

(none)

### Return value

Pointer or reference to the contained value.

### Notes

These operators do not check whether the optional contains a value! You can do
so manually by using `has_value()` or simply `operator bool()`. Alternatively,
if checked access is needed, `value()` or `value_or()` may be used.

### Example

```cpp
#include <cassert>
#include <expected>
#include <iomanip>
#include <iostream>
#include <string>

int main()
{
    using namespace std::string_literals;

    std::expected<int, std::string> ex1 = 6;
    assert(*ex1 == 6);

    *ex1 = 9;
    assert(*ex1 == 9);

    ex1 = std::unexpected("error"s);
    // *ex1 = 13 // UB, ex1 contains "unexpected" value
    assert(ex1.value_or(42) == 42);

    std::expected<std::string, bool> ex2 = "Moon"s;
    std::cout << "ex2: " << std::quoted(*ex2) << ", size: " << ex2->size() << '\n';

    // You can "take" the contained value by calling operator* on an rvalue to expected

    auto taken = *std::move(ex2);
    std::cout << "taken: " << std::quoted(taken) << "\n"
                 "ex2: " << std::quoted(*ex2) << ", size: " << ex2->size() << '\n';
}
```

Possible output:

```text
ex2: "Moon", size: 4
taken: "Moon"
ex2: "", size: 0
```

### See also

- **value** — returns the expected value (public member function)
- **value_or** — returns the expected value if present, another value otherwise
  (public member function)
- **operator boolhas_value** — checks whether the object contains an expected
  value (public member function)
- **error** — returns the unexpected value (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/expected/operator**
