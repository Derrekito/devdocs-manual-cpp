# std::basic_ofstream<CharT,Traits>::native_handle

```cpp
native_handle_type native_handle() const noexcept;  // (since C++26)
```

Returns the implementation defined underlying handle associated with
`basic_filebuf`. The behavior is undefined if `is_open()` is `false`.

### Return value

`rdbuf()->native_handle()`

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_fstream_native_handle` | 202306L | (C++26) | native handles support

### Example

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ofstream/native_handle*
