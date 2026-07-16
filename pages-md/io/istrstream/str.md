# std::istrstream::str

```cpp
char* str();
```

Returns the pointer to the beginning of the buffer, after freezing it.
Effectively calls `rdbuf()->str()`.

### Parameters

(none)

### Return value

Pointer to the beginning of the buffer in the associated `std::strsteambuf` or a
null pointer if no buffer is available.

### Example

---
*Source: https://en.cppreference.com/w/cpp/io/istrstream/str*
