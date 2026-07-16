# std::locale::~locale

```cpp
~locale();
```

Non-virtual destructor which decrements reference counts of all facets held by
`*this`. Those facets whose reference count becomes zero are deleted.

### Return value

(none)

### Example

### See also

- **(constructor)** — constructs a new locale (public member function)

---
*Source: https://en.cppreference.com/w/cpp/locale/locale/%7Elocale*
