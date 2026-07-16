# std::error_code::assign

```cpp
void assign( int val, const error_category& cat ) noexcept;  // (since C++11)
```

Replaces the contents with error code value `val` and corresponding category
`cat`.

### Parameters

- **val** — platform-dependent error code value to assign
- **cat** — error category corresponding to `val`

### Return value

(none)

### See also

- **operator=** — assigns another error code (public member function)

---
*Source: https://en.cppreference.com/w/cpp/error/error_code/assign*
