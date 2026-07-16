# std::wbuffer_convert<Codecvt,Elem,Tr>::state

```cpp
state_type state() const;
```

Returns the current value of the conversion state, which is stored in this
`wbuffer_convert` object. The conversion state may be explicitly set in the
constructor and is updated by all conversion operations.

### Return value

The current conversion state.

### Example

### See also

- **state** — returns the current conversion state (public member function of
  `std::wstring_convert<Codecvt,Elem,Wide_alloc,Byte_alloc>`)
- **mbsinit** — checks if the `std::mbstate_t` object represents initial shift
  state (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/wbuffer_convert/state*
