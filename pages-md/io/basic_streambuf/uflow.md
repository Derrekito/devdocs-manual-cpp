# std::basic_streambuf<CharT,Traits>::uflow

```cpp
protected:
virtual int_type uflow();
```

Ensures that at least one character is available in the input area by updating
the pointers to the input area (if needed). On success returns the value of that
character and advances the value of the *get pointer* by one character. On
failure returns `traits::eof()`.

The function may update `gptr`, `egptr` and `eback` pointers to define the
location of newly loaded data (if any). On failure, the function ensures that
either `gptr() == nullptr` or `gptr() == egptr`.

The base class version of the function calls `underflow()` and increments
`gptr()`.

### Parameters

(none)

### Return value

The value of the character that was pointed to by the *get pointer* before it
was advanced by one, or `traits::eof()` otherwise.

The base class version of the function returns the value returned by
`underflow()`.

### Note

The public functions of `std::streambuf` call this function only if `gptr() ==
nullptr` or `gptr() >= egptr()`.

The custom streambuf classes that do not use the get area and do not set the get
area pointers in basic_streambuf are required to override this function.

### Example

### See also

- **underflow [virtual]** — reads characters from the associated input sequence
  to the get area (virtual protected member function)
- **overflow [virtual]** — writes characters to the associated output sequence
  from the put area (virtual protected member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/uflow*
