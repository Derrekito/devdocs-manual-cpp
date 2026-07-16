# Standard library header <thread> (C++11)

This header is part of the thread support library.

**Includes**

- **<compare> (C++20)** — Three-way comparison operator support

**Namespaces**

- **`this_thread`** — provide functions that access the current thread of
  execution

**Classes**

- **thread (C++11)** — manages a separate thread (class)
- **jthread (C++20)** — `std::thread` with support for auto-joining and
  cancellation (class)
- **std::hash<std::thread::id>** — specializes `std::hash` (class template
  specialization)

**Functions**

- **std::swap(std::thread) (C++11)** — specializes the `std::swap` algorithm
  (function)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — compares two `thread::id` objects
  (function)
- **operator<<** — serializes a `thread::id` object (function template)
- **yield (C++11)** — suggests that the implementation reschedule execution of
  threads (function)
- **get_id (C++11)** — returns the thread id of the current thread (function)
- **sleep_for (C++11)** — stops the execution of the current thread for a
  specified time duration (function)
- **sleep_until (C++11)** — stops the execution of the current thread until a
  specified time point (function)

### Synopsis

```cpp
#include <compare>

namespace std {
  class thread;

  void swap(thread& x, thread& y) noexcept;

  // class jthread
  class jthread;

  namespace this_thread {
    thread::id get_id() noexcept;

    void yield() noexcept;
    template<class Clock, class Duration>
      void sleep_until(const chrono::time_point<Clock, Duration>& abs_time);
    template<class Rep, class Period>
      void sleep_for(const chrono::duration<Rep, Period>& rel_time);
  }
}
```

#### Class `std::thread`

```cpp
namespace std {
  class thread {
  public:
    // types
    class id;
    using native_handle_type = /* implementation-defined */;

    // construct/copy/destroy
    thread() noexcept;
    template<class F, class... Args> explicit thread(F&& f, Args&&... args);
    ~thread();
    thread(const thread&) = delete;
    thread(thread&&) noexcept;
    thread& operator=(const thread&) = delete;
    thread& operator=(thread&&) noexcept;

    // members
    void swap(thread&) noexcept;
    bool joinable() const noexcept;
    void join();
    void detach();
    id get_id() const noexcept;
    native_handle_type native_handle();

    // static members
    static unsigned int hardware_concurrency() noexcept;
  };
}
```

#### Class `std::jthread`

```cpp
namespace std {
  class jthread {
  public:
    // types
    using id = thread::id;
    using native_handle_type = thread::native_handle_type;

    // constructors, move, and assignment
    jthread() noexcept;
    template<class F, class... Args> explicit jthread(F&& f, Args&&... args);
    ~jthread();
    jthread(const jthread&) = delete;
    jthread(jthread&&) noexcept;
    jthread& operator=(const jthread&) = delete;
    jthread& operator=(jthread&&) noexcept;

    // members
    void swap(jthread&) noexcept;
    [[nodiscard]] bool joinable() const noexcept;
    void join();
    void detach();
    [[nodiscard]] id get_id() const noexcept;
    [[nodiscard]] native_handle_type native_handle();

    // stop token handling
    [[nodiscard]] stop_source get_stop_source() noexcept;
    [[nodiscard]] stop_token get_stop_token() const noexcept;
    bool request_stop() noexcept;

    // specialized algorithms
    friend void swap(jthread& lhs, jthread& rhs) noexcept;

    // static members
    [[nodiscard]] static unsigned int hardware_concurrency() noexcept;

  private:
    stop_source ssource;        // exposition only
  };
}
```

#### Class `std::thread::id`

```cpp
namespace std {
  class thread::id {
  public:
    id() noexcept;
  };

  bool operator==(thread::id x, thread::id y) noexcept;
  strong_ordering operator<=>(thread::id x, thread::id y) noexcept;

  template<class CharT, class Traits>
    basic_ostream<CharT, Traits>&
      operator<<(basic_ostream<CharT, Traits>& out, thread::id id);

  // hash support
  template<class T> struct hash;
  template<> struct hash<thread::id>;
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/thread*
