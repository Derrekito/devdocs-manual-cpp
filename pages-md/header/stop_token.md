# Standard library header <stop_token> (C++20)

This header is part of the thread support library.

**Classes**

- **stop_token (C++20)** — an interface for querying if a `std::jthread`
  cancellation request has been made (class)
- **stop_source (C++20)** — class representing a request to stop one or more
  `std::jthread`s (class)
- **stop_callback (C++20)** — an interface for registering callbacks on
  `std::jthread` cancellation (class template)
- **nostopstate_t (C++20)** — placeholder type for use in `stop_source`
  constructor (class)

**Constants**

- **nostopstate (C++20)** — a `std::nostopstate_t` instance for use in
  `stop_source` constructor (constant)

### Synopsis

```cpp
namespace std {
  // class stop_token
  class stop_token;

  // class stop_source
  class stop_source;

  // no-shared-stop-state indicator
  struct nostopstate_t {
    explicit nostopstate_t() = default;
  };
  inline constexpr nostopstate_t nostopstate{};

  // class stop_callback
  template<class Callback>
  class stop_callback;
}
```

#### Class `std::stop_token`

```cpp
namespace std {
  class stop_token {
  public:
    // constructors, copy, and assignment
    stop_token() noexcept;

    stop_token(const stop_token&) noexcept;
    stop_token(stop_token&&) noexcept;
    stop_token& operator=(const stop_token&) noexcept;
    stop_token& operator=(stop_token&&) noexcept;
    ~stop_token();
    void swap(stop_token&) noexcept;

    // stop handling
    [[nodiscard]] bool stop_requested() const noexcept;
    [[nodiscard]] bool stop_possible() const noexcept;

    [[nodiscard]]
    friend bool operator==(const stop_token& lhs, const stop_token& rhs) noexcept;
    friend void swap(stop_token& lhs, stop_token& rhs) noexcept;
  };
}
```

#### Class `std::stop_source`

```cpp
namespace std {
  // no-shared-stop-state indicator
  struct nostopstate_t {
    explicit nostopstate_t() = default;
  };
  inline constexpr nostopstate_t nostopstate{};

  class stop_source {
  public:
    // constructors, copy, and assignment
    stop_source();
    explicit stop_source(nostopstate_t) noexcept;

    stop_source(const stop_source&) noexcept;
    stop_source(stop_source&&) noexcept;
    stop_source& operator=(const stop_source&) noexcept;
    stop_source& operator=(stop_source&&) noexcept;
    ~stop_source();
    void swap(stop_source&) noexcept;

    // stop handling
    [[nodiscard]] stop_token get_token() const noexcept;
    [[nodiscard]] bool stop_possible() const noexcept;
    [[nodiscard]] bool stop_requested() const noexcept;
    bool request_stop() noexcept;

    [[nodiscard]] friend bool
    operator==(const stop_source& lhs, const stop_source& rhs) noexcept;
    friend void swap(stop_source& lhs, stop_source& rhs) noexcept;
  };
}
```

#### Class template `std::stop_callback`

```cpp
namespace std {
  template<class Callback>
  class stop_callback {
  public:
    using callback_type = Callback;

    // constructors and destructor
    template<class C>
    explicit stop_callback(const stop_token& st, C&& cb)
        noexcept(is_nothrow_constructible_v<Callback, C>);
    template<class C>
    explicit stop_callback(stop_token&& st, C&& cb)
        noexcept(is_nothrow_constructible_v<Callback, C>);
    ~stop_callback();

    stop_callback(const stop_callback&) = delete;
    stop_callback(stop_callback&&) = delete;
    stop_callback& operator=(const stop_callback&) = delete;
    stop_callback& operator=(stop_callback&&) = delete;

  private:
    Callback callback;      // exposition only
  };

  template<class Callback>
  stop_callback(stop_token, Callback) -> stop_callback<Callback>;
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/stop_token*
