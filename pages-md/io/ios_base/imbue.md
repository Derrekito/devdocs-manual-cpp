# std::ios_base::imbue

```cpp
std::locale imbue( const std::locale& loc );
```

Sets the associated locale of the stream to `loc`. Before returning, each
function, registered by `register_callback()` is called with `imbue_event` as a
parameter.

### Parameters

- **loc** — new locale to associate the stream to

### Return value

The locale object associated with the stream before the operation.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 47 | C++98 | the return value was misspecified as the return value of
      `getloc()` | corrected
  LWG 156 | C++98 | the type of `loc` was `const std::locale` | corrected to
      `const std::locale&`

### See also

- **getloc** — returns current locale (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/ios_base/imbue*
