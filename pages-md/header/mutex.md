# Standard library header <mutex> (C++11)

This header is part of the thread support library.

**Classes**

- **mutex (C++11)** — provides basic mutual exclusion facility (class)
- **timed_mutex (C++11)** — provides mutual exclusion facility which implements
  locking with a timeout (class)
- **recursive_mutex (C++11)** — provides mutual exclusion facility which can be
  locked recursively by the same thread (class)
- **recursive_timed_mutex (C++11)** — provides mutual exclusion facility which
  can be locked recursively by the same thread and implements locking with a
  timeout (class)
- **lock_guard (C++11)** — implements a strictly scope-based mutex ownership
  wrapper (class template)
- **unique_lock (C++11)** — implements movable mutex ownership wrapper (class
  template)
- **scoped_lock (C++17)** — deadlock-avoiding RAII wrapper for multiple mutexes
  (class template)
- **defer_lock_ttry_to_lock_tadopt_lock_t (C++11)(C++11)(C++11)** — tag type
  used to specify locking strategy (class)
- **once_flag (C++11)** — helper object to ensure that `call_once` invokes the
  function only once (class)

**Constants**

- **defer_locktry_to_lockadopt_lock (C++11)(C++11)(C++11)** — tag constants used
  to specify locking strategy (constant)

**Functions**

- **try_lock (C++11)** — attempts to obtain ownership of mutexes via repeated
  calls to `try_lock` (function template)
- **lock (C++11)** — locks specified mutexes, blocks if any are unavailable
  (function template)
- **call_once (C++11)** — invokes a function only once even if called from
  multiple threads (function template)
- **std::swap(std::unique_lock) (C++11)** — specialization of `std::swap` for
  `unique_lock` (function template)

### Synopsis

```cpp
namespace std {
  class mutex;
  class recursive_mutex;
  class timed_mutex;
  class recursive_timed_mutex;

  struct defer_lock_t { explicit defer_lock_t() = default; };
  struct try_to_lock_t { explicit try_to_lock_t() = default; };
  struct adopt_lock_t { explicit adopt_lock_t() = default; };

  inline constexpr defer_lock_t  defer_lock { };
  inline constexpr try_to_lock_t try_to_lock { };
  inline constexpr adopt_lock_t  adopt_lock { };

  template<class Mutex> class lock_guard;
  template<class... MutexTypes> class scoped_lock;
  template<class Mutex> class unique_lock;

  template<class Mutex>
    void swap(unique_lock<Mutex>& x, unique_lock<Mutex>& y) noexcept;

  template<class L1, class L2, class... L3> int try_lock(L1&, L2&, L3&...);
  template<class L1, class L2, class... L3> void lock(L1&, L2&, L3&...);

  struct once_flag;

  template<class Callable, class... Args>
    void call_once(once_flag& flag, Callable&& func, Args&&... args);
}
```

#### Class `std::mutex`

```cpp
namespace std {
  class mutex {
  public:
    constexpr mutex() noexcept;
    ~mutex();

    mutex(const mutex&) = delete;
    mutex& operator=(const mutex&) = delete;

    void lock();
    bool try_lock();
    void unlock();

    using native_handle_type = /* implementation-defined */;
    native_handle_type native_handle();
  };
}
```

#### Class `std::recursive_mutex`

```cpp
namespace std {
  class recursive_mutex {
  public:
    recursive_mutex();
    ~recursive_mutex();

    recursive_mutex(const recursive_mutex&) = delete;
    recursive_mutex& operator=(const recursive_mutex&) = delete;

    void lock();
    bool try_lock() noexcept;
    void unlock();

    using native_handle_type = /* implementation-defined */;
    native_handle_type native_handle();
  };
}
```

#### Class `std::timed_mutex`

```cpp
namespace std {
  class timed_mutex {
  public:
    timed_mutex();
    ~timed_mutex();

    timed_mutex(const timed_mutex&) = delete;
    timed_mutex& operator=(const timed_mutex&) = delete;

    void lock();    // blocking
    bool try_lock();
    template<class Rep, class Period>
      bool try_lock_for(const chrono::duration<Rep, Period>& rel_time);
    template<class Clock, class Duration>
      bool try_lock_until(const chrono::time_point<Clock, Duration>& abs_time);
    void unlock();

    using native_handle_type = /* implementation-defined */;
    native_handle_type native_handle();
  };
}
```

#### Class `std::recursive_timed_mutex`

```cpp
namespace std {
  class recursive_timed_mutex {
  public:
    recursive_timed_mutex();
    ~recursive_timed_mutex();

    recursive_timed_mutex(const recursive_timed_mutex&) = delete;
    recursive_timed_mutex& operator=(const recursive_timed_mutex&) = delete;

    void lock();    // blocking
    bool try_lock() noexcept;
    template<class Rep, class Period>
      bool try_lock_for(const chrono::duration<Rep, Period>& rel_time);
    template<class Clock, class Duration>
      bool try_lock_until(const chrono::time_point<Clock, Duration>& abs_time);
    void unlock();

    using native_handle_type = /* implementation-defined */;
    native_handle_type native_handle();
  };
}
```

#### Class template `std::lock_guard`

```cpp
namespace std {
  template<class Mutex>
  class lock_guard {
  public:
    using mutex_type = Mutex;

    explicit lock_guard(mutex_type& m);
    lock_guard(mutex_type& m, adopt_lock_t);
    ~lock_guard();

    lock_guard(const lock_guard&) = delete;
    lock_guard& operator=(const lock_guard&) = delete;

  private:
    mutex_type& pm;             // exposition only
  };
}
```

#### Class template std::scoped_lock

```cpp
namespace std {
  template<class... MutexTypes>
  class scoped_lock {
  public:
    using mutex_type = Mutex;   // If MutexTypes... consists of the single type Mutex

    explicit scoped_lock(MutexTypes&... m);
    explicit scoped_lock(adopt_lock_t, MutexTypes&... m);
    ~scoped_lock();

    scoped_lock(const scoped_lock&) = delete;
    scoped_lock& operator=(const scoped_lock&) = delete;

  private:
    tuple<MutexTypes&...> pm;   // exposition only
  };
}
```

#### Class template `std::unique_lock`

```cpp
namespace std {
  template<class Mutex>
  class unique_lock {
  public:
    using mutex_type = Mutex;

    // construct/copy/destroy
    unique_lock() noexcept;
    explicit unique_lock(mutex_type& m);
    unique_lock(mutex_type& m, defer_lock_t) noexcept;
    unique_lock(mutex_type& m, try_to_lock_t);
    unique_lock(mutex_type& m, adopt_lock_t);
    template<class Clock, class Duration>
      unique_lock(mutex_type& m, const chrono::time_point<Clock, Duration>& abs_time);
    template<class Rep, class Period>
      unique_lock(mutex_type& m, const chrono::duration<Rep, Period>& rel_time);
    ~unique_lock();

    unique_lock(const unique_lock&) = delete;
    unique_lock& operator=(const unique_lock&) = delete;

    unique_lock(unique_lock&& u) noexcept;
    unique_lock& operator=(unique_lock&& u);

    // locking
    void lock();
    bool try_lock();

    template<class Rep, class Period>
      bool try_lock_for(const chrono::duration<Rep, Period>& rel_time);
    template<class Clock, class Duration>
      bool try_lock_until(const chrono::time_point<Clock, Duration>& abs_time);

    void unlock();

    // modifiers
    void swap(unique_lock& u) noexcept;
    mutex_type* release() noexcept;

    // observers
    bool owns_lock() const noexcept;
    explicit operator bool () const noexcept;
    mutex_type* mutex() const noexcept;

  private:
    mutex_type* pm;             // exposition only
    bool owns;                  // exposition only
  };

  template<class Mutex>
    void swap(unique_lock<Mutex>& x, unique_lock<Mutex>& y) noexcept;
}
```

#### Class `std::once_flag`

```cpp
namespace std {
  struct once_flag {
    constexpr once_flag() noexcept;

    once_flag(const once_flag&) = delete;
    once_flag& operator=(const once_flag&) = delete;
  };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/mutex*
