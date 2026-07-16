# std::generator<Ref,V,Allocator>::promise_type::get_return_object

```cpp
std::generator get_return_object() noexcept;  // (since C++23)
```

Returns a generator object whose member `coroutine_` is obtained via the
expression `std::coroutine_handle<promise_type>::from_promise(*this)`, and whose
member `active_` points to an empty stack.

### Parameters

(none)

### Return value

The generator object.

---
*Source: https://en.cppreference.com/w/cpp/coroutine/generator/promise_type/get_return_object*
