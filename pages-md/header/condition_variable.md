# Standard library header <condition_variable> (C++11)

This header is part of the thread support library.

**Classes**

- **condition_variable (C++11)** — provides a condition variable associated with
  a `std::unique_lock` (class)
- **condition_variable_any (C++11)** — provides a condition variable associated
  with any lock type (class)
- **cv_status (C++11)** — lists the possible results of timed waits on condition
  variables (enum)

**Functions**

- **notify_all_at_thread_exit (C++11)** — schedules a call to `notify_all` to be
  invoked when this thread is completely finished (function)

### Synopsis

```cpp
namespace std {
  class condition_variable;
  class condition_variable_any;

  void notify_all_at_thread_exit(condition_variable& cond, unique_lock<mutex> lk);

  enum class cv_status { no_timeout, timeout };
}
```

#### Class `std::condition_variable`

```cpp
namespace std {
  class condition_variable {
  public:
    condition_variable();
    ~condition_variable();

    condition_variable(const condition_variable&) = delete;
    condition_variable& operator=(const condition_variable&) = delete;

    void notify_one() noexcept;
    void notify_all() noexcept;
    void wait(unique_lock<mutex>& lock);
    template<class Pred>
      void wait(unique_lock<mutex>& lock, Pred pred);
    template<class Clock, class Duration>
      cv_status wait_until(unique_lock<mutex>& lock,
                           const chrono::time_point<Clock, Duration>& abs_time);
    template<class Clock, class Duration, class Pred>
      bool wait_until(unique_lock<mutex>& lock,
                      const chrono::time_point<Clock, Duration>& abs_time, Pred pred);
    template<class Rep, class Period>
      cv_status wait_for(unique_lock<mutex>& lock,
                         const chrono::duration<Rep, Period>& rel_time);
    template<class Rep, class Period, class Pred>
      bool wait_for(unique_lock<mutex>& lock,
                    const chrono::duration<Rep, Period>& rel_time, Pred pred);

    using native_handle_type = /* implementation-defined */;
    native_handle_type native_handle();
  };
}
```

#### Class `std::condition_variable_any`

```cpp
namespace std {
  class condition_variable_any {
  public:
    condition_variable_any();
    ~condition_variable_any();

    condition_variable_any(const condition_variable_any&) = delete;
    condition_variable_any& operator=(const condition_variable_any&) = delete;

    void notify_one() noexcept;
    void notify_all() noexcept;

    // noninterruptible waits
    template<class Lock>
      void wait(Lock& lock);
    template<class Lock, class Pred>
      void wait(Lock& lock, Pred pred);

    template<class Lock, class Clock, class Duration>
      cv_status wait_until(Lock& lock,
                           const chrono::time_point<Clock, Duration>& abs_time);
    template<class Lock, class Clock, class Duration, class Pred>
      bool wait_until(Lock& lock, const chrono::time_point<Clock, Duration>& abs_time,
                      Pred pred);
    template<class Lock, class Rep, class Period>
      cv_status wait_for(Lock& lock, const chrono::duration<Rep, Period>& rel_time);
    template<class Lock, class Rep, class Period, class Pred>
      bool wait_for(Lock& lock, const chrono::duration<Rep, Period>& rel_time, Pred pred);

    // interruptible waits
    template<class Lock, class Pred>
      bool wait(Lock& lock, stop_token stoken, Pred pred);
    template<class Lock, class Clock, class Duration, class Pred>
      bool wait_until(Lock& lock, stop_token stoken,
                      const chrono::time_point<Clock, Duration>& abs_time, Pred pred);
    template<class Lock, class Rep, class Period, class Pred>
      bool wait_for(Lock& lock, stop_token stoken,
                    const chrono::duration<Rep, Period>& rel_time, Pred pred);
  };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/condition_variable*
