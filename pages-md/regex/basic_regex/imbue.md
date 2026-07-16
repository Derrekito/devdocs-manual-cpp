# std::basic_regex<CharT,Traits>::imbue

```cpp
locale_type imbue( locale_type loc );  // (since C++11)
```

Replaces the current locale with `loc`. The regular expression does not match
any character sequence after the call.

Effectively calls `traits_i.imbue(loc)` where `traits_i` is a default
initialized instance of the type `Traits` stored within the regular expression
object.

### Parameters

- **loc** — new locale to use

### Return value

The locale before the call to this function. Effectively returns the result of
expression `traits_i.imbue(loc)`.

### Exceptions

May throw implementation-defined exceptions.

### Example

### See also

- **getloc** — get locale information (public member function)

---
*Source: https://en.cppreference.com/w/cpp/regex/basic_regex/imbue*
