# std::basic_ios<CharT,Traits>::move

```cpp
protected:
void move( basic_ios& other );  // (since C++11)
protected:
void move( basic_ios&& other );  // (since C++11)
```

Replaces the current state with that of `other`, except for the associated
`rdbuf`. `other` is in valid, but unspecified state after the call. After the
call to this function, `rdbuf()` returns a null pointer, `other.rdbuf()` returns
the same value as before the call, and `other.tie()` returns a null pointer.

This member function is protected: it is called by the protected move
constructors of the derived stream classes `std::basic_ostream` and
`std::basic_istream`, which, in turn, are called by the public move constructors
of the further derived stream classes such as `std::basic_ofstream`, which know
how to correctly move the associated streambuffer.

### Parameters

- **other** — the `basic_ios` object to transfer the state from

### Return value

(none)

### See also

- **swap (C++11)** — swaps with another `std::basic_ios` except for `rdbuf`
  (protected member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/move*
