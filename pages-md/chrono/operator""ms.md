# std::literals::chrono_literals::operator""ms

```cpp
constexpr std::chrono::milliseconds
    operator""ms( unsigned long long ms );  // (1) (since C++14)
constexpr std::chrono::duration</*unspecified*/, std::milli>
    operator""ms( long double ms );  // (2) (since C++14)
```

Forms a `std::chrono::duration` literal representing milliseconds.

1) Integer literal, returns exactly `std::chrono::milliseconds(ms)`.

2) Floating-point literal, returns a floating-point duration equivalent to
   `std::chrono::milliseconds`.

### Parameters

- **ms** — the number of milliseconds

### Return value

The `std::chrono::duration` literal.

### Possible implementation

```cpp
constexpr std::chrono::milliseconds operator""ms(unsigned long long ms)
{
    return std::chrono::milliseconds(ms);
}
constexpr std::chrono::duration<long double, std::milli> operator""ms(long double ms)
{
    return std::chrono::duration<long double, std::milli>(ms);
}
```

### Notes

This operator is declared in the namespace `std::literals::chrono_literals`,
where both `literals` and `chrono_literals` are inline namespaces. Access to
this operator can be gained with:

- `using namespace std::literals`,
- `using namespace std::chrono_literals`, or
- `using namespace std::literals::chrono_literals`.

In addition, within the namespace `std::chrono`, the directive `using namespace
literals::chrono_literals;` is provided by the standard library, so that if a
programmer uses `using namespace std::chrono;` to gain access to the classes in
the chrono library, the corresponding literal operators become visible as well.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    using namespace std::chrono_literals;
    constexpr auto d1{250ms};
    constexpr std::chrono::milliseconds d2{1s};
    std::cout << d1 << " = " << d1.count() << " milliseconds\n"
              << d2 << " = " << d2.count() << " milliseconds\n";
}
```

Output:

```text
250ms = 250 milliseconds
1000ms = 1000 milliseconds
```

### See also

- **(constructor)** — constructs new duration (public member function of
  `std::chrono::duration<Rep,Period>`)

---
*Source: https://en.cppreference.com/w/cpp/chrono/operator%22%22ms*
