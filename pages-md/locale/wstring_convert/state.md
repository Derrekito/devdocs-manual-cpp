# std::wstring_convert<Codecvt,Elem,Wide_alloc,Byte_alloc>::state

```cpp
state_type state() const;
```

Returns the current value of the conversion state, which is stored in this
`wstring_convert` object. The conversion state may be explicitly set in the
constructor and is updated by all conversion operations.

### Return value

The current conversion state.

### Example

### See also

- **to_bytes** — converts a wide string into a byte string (public member
  function)
- **from_bytes** — converts a byte string into a wide string (public member
  function)
- **mbsinit** — checks if the `std::mbstate_t` object represents initial shift
  state (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/wstring_convert/state*
