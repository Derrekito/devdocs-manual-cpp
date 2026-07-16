# Standard library header <barrier> (C++20)

This header is part of the thread support library.

**Classes**

- **barrier (C++20)** — reusable thread barrier (class template)

### Synopsis

```cpp
namespace std {
  template<class CompletionFunction = /* see description */>
    class barrier;
}
```

#### Class template `std::barrier`

```cpp
namespace std {
  template<class CompletionFunction = /* see description */>
  class barrier {
  public:
    using arrival_token =  /* see description */;

    static constexpr ptrdiff_t max() noexcept;

    constexpr explicit barrier(ptrdiff_t expected,
                               CompletionFunction f = CompletionFunction());
    ~barrier();

    barrier(const barrier&) = delete;
    barrier& operator=(const barrier&) = delete;

    [[nodiscard]] arrival_token arrive(ptrdiff_t update = 1);
    void wait(arrival_token&& arrival) const;

    void arrive_and_wait();
    void arrive_and_drop();

  private:
    CompletionFunction completion;      // exposition only
  };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/barrier*
