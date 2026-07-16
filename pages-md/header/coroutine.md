# Standard library header <coroutine> (C++20)

This header is part of the coroutine support library.

**Includes**

- **<compare> (C++20)** — Three-way comparison operator support

**Classes**

- **coroutine_traits (C++20)** — trait type for discovering coroutine promise
  types (class template)
- **coroutine_handle (C++20)** — used to refer to a suspended or executing
  coroutine (class template)
- **std::hash<std::coroutine_handle> (C++20)** — hash support for
  `std::coroutine_handle` (class template specialization)

**No-op Coroutines**

- **noop_coroutine_promise (C++20)** — used for coroutines with no observable
  effects (class)
- **noop_coroutine_handle (C++20)** —
  `std::coroutine_handle<std::noop_coroutine_promise>`, intended to refer to a
  no-op coroutine (typedef)

**Trivial Awaitables**

- **suspend_never (C++20)** — indicates that an await-expression should never
  suspend (class)
- **suspend_always (C++20)** — indicates that an await-expression should always
  suspend (class)

**Functions**

- **operator==operator<=> (C++20)** — compares two `coroutine_handle` objects
  (function)

**No-op Coroutines**

- **noop_coroutine (C++20)** — creates a coroutine handle that has no observable
  effects when resumed or destroyed (function)

### Synopsis

```cpp
#include <compare>

namespace std {
  // coroutine traits
  template<class R, class... ArgTypes>
    struct coroutine_traits;

  // coroutine handle
  template<class Promise = void>
    struct coroutine_handle;

  // comparison operators
  constexpr bool operator==(coroutine_handle<> x, coroutine_handle<> y) noexcept;
  constexpr strong_ordering operator<=>(coroutine_handle<> x, coroutine_handle<> y) noexcept;

  // hash support
  template<class T> struct hash;
  template<class P> struct hash<coroutine_handle<P>>;

  // no-op coroutines
  struct noop_coroutine_promise;

  template<> struct coroutine_handle<noop_coroutine_promise>;
  using noop_coroutine_handle = coroutine_handle<noop_coroutine_promise>;

  noop_coroutine_handle noop_coroutine() noexcept;

  // trivial awaitables
  struct suspend_never;
  struct suspend_always;
}
```

#### Class template `std::coroutine_handle`

```cpp
namespace std {
  template<>
  struct coroutine_handle<void>
  {
    // construct/reset
    constexpr coroutine_handle() noexcept;
    constexpr coroutine_handle(nullptr_t) noexcept;
    coroutine_handle& operator=(nullptr_t) noexcept;

    // export/import
    constexpr void* address() const noexcept;
    static constexpr coroutine_handle from_address(void* addr);

    // observers
    constexpr explicit operator bool() const noexcept;
    bool done() const;

    // resumption
    void operator()() const;
    void resume() const;
    void destroy() const;

  private:
    void* ptr;  // exposition only
  };

  template<class Promise>
  struct coroutine_handle
  {
    // construct/reset
    constexpr coroutine_handle() noexcept;
    constexpr coroutine_handle(nullptr_t) noexcept;
    static coroutine_handle from_promise(Promise&);
    coroutine_handle& operator=(nullptr_t) noexcept;

    // export/import
    constexpr void* address() const noexcept;
    static constexpr coroutine_handle from_address(void* addr);

    // conversion
    constexpr operator coroutine_handle<>() const noexcept;

    // observers
    constexpr explicit operator bool() const noexcept;
    bool done() const;

    // resumption
    void operator()() const;
    void resume() const;
    void destroy() const;

    // promise access
    Promise& promise() const;

  private:
    void* ptr;  // exposition only
  };
}
```

#### Class `std::noop_coroutine_promise`

```cpp
namespace std {
  struct noop_coroutine_promise {};
}
```

#### Class std::coroutine_handle<std::noop_coroutine_promise>

```cpp
namespace std {
  template<>
  struct coroutine_handle<noop_coroutine_promise>
  {
    // conversion
    constexpr operator coroutine_handle<>() const noexcept;

    // observers
    constexpr explicit operator bool() const noexcept;
    constexpr bool done() const noexcept;

    // resumption
    constexpr void operator()() const noexcept;
    constexpr void resume() const noexcept;
    constexpr void destroy() const noexcept;

    // promise access
    noop_coroutine_promise& promise() const noexcept;

    // address
    constexpr void* address() const noexcept;
  private:
    coroutine_handle(/* unspecified */);
    void* ptr; // exposition only
  };
}
```

#### Class `std::suspend_never`

```cpp
namespace std {
  struct suspend_never {
    constexpr bool await_ready() const noexcept { return true; }
    constexpr void await_suspend(coroutine_handle<>) const noexcept {}
    constexpr void await_resume() const noexcept {}
  };
}
```

#### Class `std::suspend_always`

```cpp
namespace std {
  struct suspend_always {
    constexpr bool await_ready() const noexcept { return false; }
    constexpr void await_suspend(coroutine_handle<>) const noexcept {}
    constexpr void await_resume() const noexcept {}
  };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/coroutine*
