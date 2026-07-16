# std::suspend_never

```cpp
struct suspend_never;  // (since C++20)
```

`suspend_never` is an empty class which can be used to indicate that an await
expression never suspends and does not produce a value.

### Member functions

- **await_ready (C++20)** — indicates that an await expression never suspends
  (public member function)
- **await_suspend (C++20)** — no-op (public member function)
- **await_resume (C++20)** — no-op (public member function)

## std::suspend_never::await_ready

```cpp
constexpr bool await_ready() const noexcept { return true; }  // (since C++20)
```

Always returns `true`, indicating that an await expression never suspends.

## std::suspend_never::await_suspend

```cpp
constexpr void await_suspend( std::coroutine_handle<> ) const noexcept {}  // (since C++20)
```

Does nothing.

## std::suspend_never::await_resume

```cpp
constexpr void await_resume() const noexcept {}  // (since C++20)
```

Does nothing. An await expression does not produce a value if `suspend_never` is
used.

### Example

### See also

- **suspend_always (C++20)** — indicates that an await-expression should always
  suspend (class)

---
*Source: https://en.cppreference.com/w/cpp/coroutine/suspend_never*
