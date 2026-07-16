# Standard library header <latch> (C++20)

This header is part of the thread support library.

**Classes**

- **latch (C++20)** — single-use thread barrier (class)

### Synopsis

```cpp
namespace std {
  class latch;
}
```

#### Class `std::latch`

```cpp
namespace std {
  class latch {
  public:
    static constexpr ptrdiff_t max() noexcept;

    constexpr explicit latch(ptrdiff_t expected);
    ~latch();

    latch(const latch&) = delete;
    latch& operator=(const latch&) = delete;

    void count_down(ptrdiff_t update = 1);
    bool try_wait() const noexcept;
    void wait() const;
    void arrive_and_wait(ptrdiff_t update = 1);

  private:
    ptrdiff_t counter;  // exposition only
  };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/latch*
