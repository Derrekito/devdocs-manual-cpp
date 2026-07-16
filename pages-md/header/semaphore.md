# Standard library header <semaphore> (C++20)

This header is part of the thread support library.

**Classes**

- **counting_semaphore (C++20)** — semaphore that models a non-negative resource
  count (class template)
- **binary_semaphore (C++20)** — semaphore that has only two states (typedef)

### Synopsis

```cpp
namespace std {
  template<ptrdiff_t LeastMaxValue = /* implementation-defined */>
    class counting_semaphore;

  using binary_semaphore = counting_semaphore<1>;
}
```

#### Class template `std::counting_semaphore`

```cpp
namespace std {
  template<ptrdiff_t LeastMaxValue = /* implementation-defined */>
  class counting_semaphore {
  public:
    static constexpr ptrdiff_t max() noexcept;

    constexpr explicit counting_semaphore(ptrdiff_t desired);
    ~counting_semaphore();

    counting_semaphore(const counting_semaphore&) = delete;
    counting_semaphore& operator=(const counting_semaphore&) = delete;

    void release(ptrdiff_t update = 1);
    void acquire();
    bool try_acquire() noexcept;
    template<class Rep, class Period>
      bool try_acquire_for(const chrono::duration<Rep, Period>& rel_time);
    template<class Clock, class Duration>
      bool try_acquire_until(const chrono::time_point<Clock, Duration>& abs_time);

  private:
    ptrdiff_t counter;          // exposition only
  };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/semaphore*
