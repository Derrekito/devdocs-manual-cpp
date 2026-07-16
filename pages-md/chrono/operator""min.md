# std::literals::chrono_literals::operator""min

```cpp
constexpr chrono::minutes
    operator""min( unsigned long long mins );  // (1) (since C++14)
constexpr chrono::duration</*unspecified*/, ratio<60,1>>
    operator""min( long double mins );  // (2) (since C++14)
```

Forms a `std::chrono::duration` literal representing minutes.

1) Integer literal, returns exactly `std::chrono::minutes(mins)`.

2) Floating-point literal, returns a floating-point duration equivalent to
   `std::chrono::minutes`.

### Parameters

- **mins** — the number of minutes

### Return value

The `std::chrono::duration` literal.

### Possible implementation

```cpp
constexpr std::chrono::minutes operator""min(unsigned long long m)
{
    return std::chrono::minutes(m);
}
constexpr std::chrono::duration<long double,
                                std::ratio<60,1>> operator""min(long double m)
{
    return std::chrono::duration<long double, ratio<60,1>> (m);
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
    auto lesson = 45min;
    auto halfmin = 0.5min;
    std::cout << "One lesson is " << lesson.count() << " minutes"
                 " (" << lesson << ")\n"
              << "Half a minute is " << halfmin.count() << " minutes"
                 " (" << halfmin << ")\n";
}
```

Output:

```text
One lesson is 45 minutes (45min)
Half a minute is 0.5 minutes (0.5min)
```

### See also

- **(constructor)** — constructs new duration (public member function of
  `std::chrono::duration<Rep,Period>`)

---
*Source: https://en.cppreference.com/w/cpp/chrono/operator%22%22min*
