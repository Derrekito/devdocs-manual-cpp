# std::basic_stringbuf<CharT,Traits,Allocator>::view

```cpp
std::basic_string_view<CharT, Traits> view() const noexcept;  // (since C++20)
```

Creates and returns a `std::basic_string_view` over the underlying character
sequence.

- For input-only streams, the returned view represents the range `[eback(),
  egptr())`.
- For input/output or output-only streams, references the characters from
  `pbase()` to the last character in the sequence regardless of `egptr()` and
  `epptr()`.
- If the stream is neither input nor output, returns
  `std::basic_string_view<CharT, Traits>{}`.

The member character sequence in a buffer open for writing can be over-allocated
for efficiency purposes. In that case, only the *initialized characters* are
referenced: these characters are the ones that were obtained from the string
argument of the constructor, the string argument used by the most recent call to
a setter overload of `str()`, or from a write operation. A typical
implementation that uses over-allocation maintains a high-watermark pointer to
track the end of the initialized part of the buffer and the returned view
references the characters from `pbase()` to the high-watermark pointer.

### Parameters

(none)

### Return value

A view over the underlying character sequence.

### Notes

Using of the return value is undefined behavior if the underlying character
sequence is destroyed or invalidated, unless the return value is empty.

This function is typically accessed through `std::basic_istringstream::view()`,
`std::basic_ostringstream::view()`, or `std::basic_stringstream::view()`.

### See also

- **str** — replaces or obtains a copy of the associated character string
  (public member function)
- **operator basic_string_view (C++17)** — returns a non-modifiable string_view
  into the entire string (public member function of
  `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_stringbuf/view*
