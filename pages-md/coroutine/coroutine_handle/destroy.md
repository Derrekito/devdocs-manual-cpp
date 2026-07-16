# std::coroutine_handle<Promise>::destroy

```cpp
Member of other specializations
void destroy() const;  // (1) (since C++20)
Member of specialization std::coroutine_handle<std::noop_coroutine_promise>
constexpr void destroy() const noexcept;  // (2) (since C++20)
```

1) Destroys the coroutine state of the coroutine to which `*this` refers, or
   does nothing if the coroutine is a no-op coroutine.

2) Does nothing.

The behavior is undefined if destroying is needed and `*this` does not refer to
a suspended coroutine.

### Parameters

(none)

### Return value

(none)

### Example

### See also

- **operator()resume (C++20)** — resumes execution of the coroutine (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/coroutine/coroutine_handle/destroy*
