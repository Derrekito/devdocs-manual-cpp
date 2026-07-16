# std::optional<T>::operator->, std::optional<T>::operator*

```cpp
constexpr const T* operator->() const noexcept;  // (1) (since C++17)
constexpr T* operator->() noexcept;  // (1) (since C++17)
constexpr const T& operator*() const& noexcept;  // (2) (since C++17)
constexpr T& operator*() & noexcept;  // (2) (since C++17)
constexpr const T&& operator*() const&& noexcept;  // (2) (since C++17)
constexpr T&& operator*() && noexcept;  // (2) (since C++17)
```

Accesses the contained value.

1) Returns a pointer to the contained value.

2) Returns a reference to the contained value.

The behavior is undefined if `*this` does not contain a value.

### Parameters

(none)

### Return value

Pointer or reference to the contained value.

### Notes

This operator does not check whether the optional contains a value! You can do
so manually by using `has_value()` or simply `operator bool()`. Alternatively,
if checked access is needed, `value()` or `value_or()` may be used.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <optional>
#include <string>

int main()
{
    using namespace std::string_literals;

    std::optional<int> opt1 = 1;
    std::cout << "opt1: " << *opt1 << '\n';

    *opt1 = 2;
    std::cout << "opt1: " << *opt1 << '\n';

    std::optional<std::string> opt2 = "abc"s;
    std::cout << "opt2: " << std::quoted(*opt2) << ", size: " << opt2->size() << '\n';

    // You can "take" the contained value by calling operator* on an rvalue to optional

    auto taken = *std::move(opt2);
    std::cout << "taken: " << std::quoted(taken) << "\n"
                 "opt2: " << std::quoted(*opt2) << ", size: " << opt2->size()  << '\n';
}
```

Output:

```text
opt1: 1
opt1: 2
opt2: "abc", size: 3
taken: "abc"
opt2: "", size: 0
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2762 | C++17 | `operator->` and `operator*` might be potentially-throwing
      | made noexcept

### See also

- **value** — returns the contained value (public member function)
- **value_or** — returns the contained value if available, another value
  otherwise (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/optional/operator**
