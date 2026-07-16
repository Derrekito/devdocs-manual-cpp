# std::literals::chrono_literals Symbol Index

This page tries to list all the symbols that are available from the standard
library (date and time library) in the namespace
`std::literals::chrono_literals`. The symbols are written as follows:

- Function names with `()`.
- Templates with `<>`.

### Notes

These operators are declared in the namespace `std::literals::chrono_literals`,
where both `literals` and `chrono_literals` are inline namespaces. Access to
these operators can be gained with:

- `using namespace std::literals`,
- `using namespace std::chrono_literals`, or
- `using namespace std::literals::chrono_literals`.

In addition, within the namespace `std::chrono`, the directive `using namespace
literals::chrono_literals;` is provided by the standard library, so that if a
programmer uses `using namespace std::chrono;` to gain access to the classes in
the chrono library, the corresponding literal operators become visible as well.

## `D H M N S U Y`

### D

`d` (since C++20)

### H

`h` (since C++14)

### M

`min` (since C++14)
`ms` (since C++14)

### N

`ns` (since C++14)

### S

`s` (since C++14)

### U

`us` (since C++14)

### Y

`y` (since C++20)

---
*Source: https://en.cppreference.com/w/cpp/symbol_index/chrono_literals*
