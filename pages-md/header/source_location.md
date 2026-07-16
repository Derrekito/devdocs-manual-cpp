# Standard library header <source_location> (C++20)

This header is part of the utility library.

**Classes**

- **source_location (C++20)** — a class representing information about the
  source code, such as file names, line numbers, and function names (class)

### Synopsis

```cpp
namespace std {
  struct source_location;
}
```

#### Class `std::source_location`

```cpp
namespace std {
  struct source_location {
    // source location construction
    static consteval source_location current() noexcept;
    constexpr source_location() noexcept;

    // source location field access
    constexpr uint_least32_t line() const noexcept;
    constexpr uint_least32_t column() const noexcept;
    constexpr const char* file_name() const noexcept;
    constexpr const char* function_name() const noexcept;

  private:
    uint_least32_t line_;               // exposition only
    uint_least32_t column_;             // exposition only
    const char* file_name_;             // exposition only
    const char* function_name_;         // exposition only
  };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/source_location*
