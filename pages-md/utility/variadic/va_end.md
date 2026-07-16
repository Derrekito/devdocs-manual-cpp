# va_end

```cpp
void va_end( std::va_list ap );
```

The `va_end` macro performs cleanup for an `ap` object initialized by a call to
`va_start` or `va_copy`. `va_end` may modify `ap` so that it is no longer
usable.

If there is no corresponding call to `va_start` or `va_copy`, or if `va_end` is
not called before a function that calls `va_start` or `va_copy` returns, the
behavior is undefined.

### Parameters

- **ap** — an instance of the `va_list` type to clean up

### Expanded value

(none)

### See also

- **va_start** — enables access to variadic function arguments (function macro)
- **va_copy (C++11)** — makes a copy of the variadic function arguments
  (function macro)
- **va_arg** — accesses the next variadic function argument (function macro)

**C documentation for `va_end`**

---
*Source: https://en.cppreference.com/w/cpp/utility/variadic/va_end*
