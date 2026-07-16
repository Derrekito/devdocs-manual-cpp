# std::generator<Ref,V,Allocator>::promise_type::return_void

```cpp
void return_void() const noexcept  // (since C++23)
```

No-op. Equivalent to `{}`. A user provided coroutine that uses the generator
cannot issue a value via co_return operator or reaching the end of the coroutine
body.

### Parameters

(none)

### Return value

(none)

---
*Source: https://en.cppreference.com/w/cpp/coroutine/generator/promise_type/return_void*
