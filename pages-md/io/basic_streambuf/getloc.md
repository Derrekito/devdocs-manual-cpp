# std::basic_streambuf<CharT,Traits>::getloc

```cpp
std::locale getloc() const;
```

Returns the associated locale.

The associated locale is the value supplied to `pubimbue()` on the last call,
or, if that function has not been called, the value of the global locale
(`std::locale`) at the time of the construction of the streambuf.

### Parameters

(none)

### Return value

The associated locale.

### Example

### See also

- **pubimbue** — invokes `imbue()` (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/getloc*
