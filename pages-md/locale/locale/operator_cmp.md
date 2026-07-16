# std::locale::operator==, operator!=

```cpp
bool operator==( const locale& other ) const;  // (1)
bool operator!=( const locale& other ) const;  // (2) (until C++20)
```

Tests two locales for equality. Named locales are considered equal if their
names are equal. Unnamed locales are considered equal if they are copies of each
other.

The `!=` operator is synthesized from `operator==`.
*(since C++20)*

### Parameters

- **other** — a `std::locale` object to compare

### Return value

1) `true` if `other` is a copy of `*this` or has an identical name, `false`
   otherwise.

2) `false` if `other` is a copy of `*this` or has an identical name, `true`
   otherwise.

### Example

### See also

- **(constructor)** — constructs a new locale (public member function)
- **name** — returns the name of the locale or "*" if unnamed (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/locale/locale/operator_cmp*
