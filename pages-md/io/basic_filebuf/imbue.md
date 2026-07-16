# std::basic_filebuf<CharT,Traits>::imbue

```cpp
protected:
virtual void imbue( const std::locale& loc )
```

Changes the associated locale so that all characters inserted or extracted after
this call (and until another call to `imbue()`) are converted using the
`std::codecvt` facet of `loc`.

If the old locale's encoding is state-dependent and file is not positioned at
the beginning, then the new locale must have the same `std::codecvt` facet as
the one previously imbued.

### Parameters

- **loc** — the locale to imbue the stream with

### Return value

(none)

### Example

### See also

- **imbue [virtual]** — changes the associated locale (virtual protected member
  function of `std::basic_streambuf<CharT,Traits>`)
- **imbue** — sets the locale (public member function of
  `std::basic_ios<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_filebuf/imbue*
