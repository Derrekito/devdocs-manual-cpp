# std::shared_future<T>::get

```cpp
const T& get() const;  // (1) (member only of generic shared_future template)(since C++11)
T& get() const;  // (2) (member only of shared_future<T&> template specialization)(since C++11)
void get() const;  // (3) (member only of shared_future<void> template specialization)(since C++11)
```

The `get` member function waits until the `shared_future` has a valid result and
(depending on which template is used) retrieves it. It effectively calls
`wait()` in order to wait for the result.

The generic template and two template specializations each contain a single
version of `get`. The three versions of `get` differ only in the return type.

The behavior is undefined if `valid()` is `false` before the call to this
function.

### Parameters

(none)

### Return value

1) Const reference to the value stored in the shared state. Accessing the value
   through this reference is undefined after the shared state has been
   destroyed.

2) The reference stored as value in the shared state.

3) Nothing.

### Exceptions

If an exception was stored in the shared state referenced by the future (e.g.
via a call to `std::promise::set_exception()`) then that exception will be
thrown.

### Notes

The implementations are encouraged to detect the case when `valid()` is `false`
before the call and throw a `std::future_error` with an error condition of
`std::future_errc::no_state`.

### Example

### See also

- **valid** — checks if the future has a shared state (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_future/get*
