# std::literals::string_literals Symbol Index

This page tries to list all the symbols that are available from the standard
library in the namespace `std::literals::string_literals`. The symbols are
written as follows:

- Function names with `()`.
- Templates with `<>`.

### Notes

These operators are declared in the namespace `std::literals::string_literals`,
where both `literals` and `string_literals` are inline namespaces. Access to
these operators can be gained with:

- `using namespace std::literals`,
- `using namespace std::string_literals`, or
- `using namespace std::literals::string_literals`.

`std::chrono::duration` also defines operator""s to represent literal seconds,
but it is an arithmetic literal: `10.0s` and `10s` are ten seconds, but `"10"s`
is a string.

## `S`

### S

`s` (since C++14)

---
*Source: https://en.cppreference.com/w/cpp/symbol_index/string_literals*
