# std::suspend_always

```cpp
struct suspend_always;  // (since C++20)
```

`suspend_always` is an empty class which can be used to indicate that an await
expression always suspends and does not produce a value.

### Member functions

- **await_ready (C++20)** — indicates that an await expression always suspends
  (public member function)
- **await_suspend (C++20)** — no-op (public member function)
- **await_resume (C++20)** — no-op (public member function)

## std::suspend_always::await_ready

```cpp
constexpr bool await_ready() const noexcept { return false; }  // (since C++20)
```

Always returns `false`, indicating that an await expression always suspends.

## std::suspend_always::await_suspend

```cpp
constexpr void await_suspend( std::coroutine_handle<> ) const noexcept {}  // (since C++20)
```

Does nothing.

## std::suspend_always::await_resume

```cpp
constexpr void await_resume() const noexcept {}  // (since C++20)
```

Does nothing. An await expression does not produce a value if `suspend_always`
is used.

### Example

### See also

- **suspend_never (C++20)** — indicates that an await-expression should never
  suspend (class)

---
*Source: https://en.cppreference.com/w/cpp/coroutine/suspend_always*
