# std::basic_streambuf<CharT,Traits>::pbase, std::basic_streambuf<CharT,Traits>::pptr, std::basic_streambuf<CharT,Traits>::epptr

```cpp
protected:
char_type* pbase() const;  // (1)
protected:
char_type* pptr() const;  // (2)
protected:
char_type* epptr() const;  // (3)
```

Returns pointers defining the put area.

1) Returns the pointer to the beginning ("base") of the put area.

2) Returns the pointer to the current character (*put pointer*) in the put area.

3) Returns the pointer one past the end of the put area.

### Parameters

(none)

### Return value

1) The pointer to the beginning of the put area.

2) The pointer to the current character (*put pointer*) in the put area.

3) The pointer one past the end of the put area.

### Example

### See also

- **ebackgptregptr** — returns a pointer to the beginning, current character and
  the end of the get area (protected member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/pptr*
