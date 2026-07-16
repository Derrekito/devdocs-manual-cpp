# std::basic_regex<CharT,Traits>::getloc

```cpp
locale_type getloc() const;  // (since C++11)
```

Returns the current locale associated with the regular expression.

Effectively calls `traits_i.getloc()` where `traits_i` is a default initialized
instance of the type `Traits`, stored within the regular expression object.

### Parameters

(none)

### Return value

The current locale associated with the regular expression.

### Exceptions

May throw implementation-defined exceptions.

### Example

### See also

- **imbue** — set locale information (public member function)

---
*Source: https://en.cppreference.com/w/cpp/regex/basic_regex/getloc*
