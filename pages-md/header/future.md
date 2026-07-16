# Standard library header <future> (C++11)

This header is part of the thread support library.

**Classes**

- **promise (C++11)** — stores a value for asynchronous retrieval (class
  template)
- **packaged_task (C++11)** — packages a function to store its return value for
  asynchronous retrieval (class template)
- **future (C++11)** — waits for a value that is set asynchronously (class
  template)
- **shared_future (C++11)** — waits for a value (possibly referenced by other
  futures) that is set asynchronously (class template)
- **launch (C++11)** — specifies the launch policy for `std::async` (enum)
- **future_status (C++11)** — specifies the results of timed waits performed on
  `std::future` and `std::shared_future` (enum)
- **future_error (C++11)** — reports an error related to futures or promises
  (class)
- **future_errc (C++11)** — identifies the future error codes (enum)
- **std::uses_allocator<std::promise> (C++11)** — specializes the
  `std::uses_allocator` type trait (class template specialization)
- **std::uses_allocator<std::packaged_task> (C++11) (until C++17)** —
  specializes the `std::uses_allocator` type trait (class template
  specialization)

**Functions**

- **async (C++11)** — runs a function asynchronously (potentially in a new
  thread) and returns a `std::future` that will hold the result (function
  template)
- **future_category (C++11)** — identifies the future error category (function)
- **std::swap(std::promise) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **std::swap(std::packaged_task) (C++11)** — specializes the `std::swap`
  algorithm (function template)

### Synopsis

```cpp
namespace std {
  enum class future_errc {
    broken_promise = /* implementation-defined */,
    future_already_retrieved = /* implementation-defined */,
    promise_already_satisfied = /* implementation-defined */,
    no_state = /* implementation-defined */
  };

  enum class launch : /* unspecified */ {
    async = /* unspecified */,
    deferred = /* unspecified */,
    /* implementation-defined */
  };

  enum class future_status {
    ready,
    timeout,
    deferred
  };

  template<> struct is_error_code_enum<future_errc> : public true_type { };
  error_code make_error_code(future_errc e) noexcept;
  error_condition make_error_condition(future_errc e) noexcept;

  const error_category& future_category() noexcept;

  class future_error;

  template<class R> class promise;
  template<class R> class promise<R&>;
  template<> class promise<void>;

  template<class R>
    void swap(promise<R>& x, promise<R>& y) noexcept;

  template<class R, class Alloc>
    struct uses_allocator<promise<R>, Alloc>;

  template<class R> class future;
  template<class R> class future<R&>;
  template<> class future<void>;

  template<class R> class shared_future;
  template<class R> class shared_future<R&>;
  template<> class shared_future<void>;

  template<class> class packaged_task;  // not defined
  template<class R, class... ArgTypes>
    class packaged_task<R(ArgTypes...)>;

  template<class R, class... ArgTypes>
    void swap(packaged_task<R(ArgTypes...)>&, packaged_task<R(ArgTypes...)>&) noexcept;

  template<class F, class... Args>
    [[nodiscard]] future<invoke_result_t<decay_t<F>, decay_t<Args>...>>
      async(F&& f, Args&&... args);
  template<class F, class... Args>
    [[nodiscard]] future<invoke_result_t<decay_t<F>, decay_t<Args>...>>
      async(launch policy, F&& f, Args&&... args);
}
```

#### Class `std::future_error`

```cpp
namespace std {
  class future_error : public logic_error {
  public:
    explicit future_error(future_errc e);

    const error_code& code() const noexcept;
    const char*       what() const noexcept;

  private:
    error_code ec_;             // exposition only
  };
}
```

#### Class template `std::promise`

```cpp
namespace std {
  template<class R>
  class promise {
  public:
    promise();
    template<class Allocator>
      promise(allocator_arg_t, const Allocator& a);
    promise(promise&& rhs) noexcept;
    promise(const promise&) = delete;
    ~promise();

    // assignment
    promise& operator=(promise&& rhs) noexcept;
    promise& operator=(const promise&) = delete;
    void swap(promise& other) noexcept;

    // retrieving the result
    future<R> get_future();

    // setting the result
    void set_value(/* see description */);
    void set_exception(exception_ptr p);

    // setting the result with deferred notification
    void set_value_at_thread_exit(/* see description */);
    void set_exception_at_thread_exit(exception_ptr p);
  };

  template<class R>
    void swap(promise<R>& x, promise<R>& y) noexcept;

  template<class R, class Alloc>
    struct uses_allocator<promise<R>, Alloc>;
}
```

#### Class template `std::future`

```cpp
namespace std {
  template<class R>
  class future {
  public:
    future() noexcept;
    future(future&&) noexcept;
    future(const future&) = delete;
    ~future();
    future& operator=(const future&) = delete;
    future& operator=(future&&) noexcept;
    shared_future<R> share() noexcept;

    // retrieving the value
    /* see description */ get();

    // functions to check state
    bool valid() const noexcept;

    void wait() const;
    template<class Rep, class Period>
      future_status wait_for(const chrono::duration<Rep, Period>& rel_time) const;
    template<class Clock, class Duration>
      future_status wait_until(const chrono::time_point<Clock, Duration>& abs_time) const;
  };
}
```

#### Class template `std::shared_future`

```cpp
namespace std {
  template<class R>
  class shared_future {
  public:
    shared_future() noexcept;
    shared_future(const shared_future& rhs) noexcept;
    shared_future(future<R>&&) noexcept;
    shared_future(shared_future&& rhs) noexcept;
    ~shared_future();
    shared_future& operator=(const shared_future& rhs) noexcept;
    shared_future& operator=(shared_future&& rhs) noexcept;

    // retrieving the value
    /* see description */ get() const;

    // functions to check state
    bool valid() const noexcept;

    void wait() const;
    template<class Rep, class Period>
      future_status wait_for(const chrono::duration<Rep, Period>& rel_time) const;
    template<class Clock, class Duration>
      future_status wait_until(const chrono::time_point<Clock, Duration>& abs_time) const;
  };
}
```

#### Class template `std::packaged_task`

```cpp
namespace std {
  template<class> class packaged_task;  // not defined

  template<class R, class... ArgTypes>
  class packaged_task<R(ArgTypes...)> {
  public:
    // construction and destruction
    packaged_task() noexcept;
    template<class F>
      explicit packaged_task(F&& f);
    ~packaged_task();

    // no copy
    packaged_task(const packaged_task&) = delete;
    packaged_task& operator=(const packaged_task&) = delete;

    // move support
    packaged_task(packaged_task&& rhs) noexcept;
    packaged_task& operator=(packaged_task&& rhs) noexcept;
    void swap(packaged_task& other) noexcept;

    bool valid() const noexcept;

    // result retrieval
    future<R> get_future();

    // execution
    void operator()(ArgTypes... );
    void make_ready_at_thread_exit(ArgTypes...);

    void reset();
  };

  template<class R, class... ArgTypes>
  packaged_task(R (*)(ArgTypes...)) -> packaged_task<R(ArgTypes...)>;

  template<class F> packaged_task(F) -> packaged_task</* see description */>;

  template<class R, class... ArgTypes>
    void swap(packaged_task<R(ArgTypes...)>& x, packaged_task<R(ArgTypes...)>& y) noexcept;
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/future*
