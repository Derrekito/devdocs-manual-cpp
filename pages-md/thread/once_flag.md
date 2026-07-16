# std::once_flag

```cpp
class once_flag;  // (since C++11)
```

The class `std::once_flag` is a helper structure for `std::call_once`.

An object of type `std::once_flag` that is passed to multiple calls to
`std::call_once` allows those calls to coordinate with each other such that only
one of the calls will actually run to completion.

`std::once_flag` is neither copyable nor movable.

### Member functions

## std::once_flag::once_flag

```cpp
constexpr once_flag() noexcept;
```

Constructs an `once_flag` object. The internal state is set to indicate that no
function has been called yet.

### Parameters

(none)

### See also

- **call_once (C++11)** — invokes a function only once even if called from
  multiple threads (function template)

**C documentation for `once_flag`**

---
*Source: https://en.cppreference.com/w/cpp/thread/once_flag*
