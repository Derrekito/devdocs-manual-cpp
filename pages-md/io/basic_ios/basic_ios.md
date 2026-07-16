# std::basic_ios<CharT,Traits>::basic_ios

```cpp
protected:
basic_ios();  // (1)
public:
explicit basic_ios( std::basic_streambuf<CharT, Traits>* sb );  // (2)
private:
basic_ios( const basic_ios& );  // (until C++11)
public:
basic_ios( const basic_ios& ) = delete;  // (since C++11)
```

Constructs a new `std::basic_ios` object.

1) Default constructor. The internal state is not initialized. `init()` must be
   called before the first use of the object or before destructor, otherwise the
   behavior is undefined.

2) Initializes the internal state by calling `init(sb)`. The associated stream
   buffer is set to `sb`.

3) The copy constructor is declared private and not defined(until
   C++11)explicitly defined as deleted(since C++11): I/O streams are not
   CopyConstructible.

### Parameters

- **sb** — stream buffer to associate to

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 1249 | C++98 | internal state did not need to be initialized before first
      use | also needs to be initialized

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/basic_ios*
