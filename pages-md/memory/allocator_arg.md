# std::allocator_arg

```cpp
struct allocator_arg_t { explicit allocator_arg_t() = default; };  // (since C++11)
constexpr std::allocator_arg_t allocator_arg = std::allocator_arg_t();  // (since C++11) (until C++17)
inline constexpr std::allocator_arg_t allocator_arg = std::allocator_arg_t();  // (since C++17)
```

`std::allocator_arg_t` is an empty class type used to disambiguate the overloads
of constructors and member functions of allocator-aware objects, including
`std::tuple`, `std::function`, `std::packaged_task`,(until C++17) and
`std::promise`. `std::allocator_arg` is a constant of it.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2510 | C++11 | the default constructor was non-explicit, which could lead
      to ambiguity | made explicit

### See also

- **uses_allocator (C++11)** — checks if the specified type supports
  uses-allocator construction (class template)

---
*Source: https://en.cppreference.com/w/cpp/memory/allocator_arg*
