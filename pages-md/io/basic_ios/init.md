# std::basic_ios<CharT,Traits>::init

```cpp
protected:
void init( std::basic_streambuf<CharT,Traits>* sb );
```

Sets the associated stream buffer to `sb` and initializes the internal state.

The postconditions are as follows:

  Element | Value
  `rdbuf()` | `sb`
  `tie()` | null pointer
  `rdstate()` | `goodbit` if `sb` is not a null pointer, otherwise `badbit`
  `exceptions()` | `goodbit`
  `flags()` | `skipws | dec`
  `width()` | `​0​`
  `precision()` | `6`
  `fill()` | `widen(' ')`
  `getloc()` | a copy of the value returned by `std::locale()`

This member function is protected: it is called by the constructors of the
derived stream classes `std::basic_istream` and `std::basic_ostream` once the
associated stream buffer is known. Until this function is called, every member
function (including the destructor) of the default-constructed `std::basic_ios`
invokes undefined behavior. Note that `basic_ios` is a virtual base class, and
therefore its constructor is not called by the constructors of those directly
derived classes, which is why two-stage initialization is necessary.

### Parameters

- **sb** — stream buffer to associate to

### See also

- **(constructor)** — constructs the object (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/init*
