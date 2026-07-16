# std::ios_base::ios_base

```cpp
private:
ios_base( const ios_base& );  // (until C++11)
public:
ios_base( const ios_base& ) = delete;  // (since C++11)
protected:
ios_base();  // (2)
```

1) The copy constructor is private(until C++11)deleted(since C++11): streams are
   not copyable.

2) The default constructor is protected: only derived classes may construct
   `std::ios_base`. The internal state is undefined after the construction. The
   derived class must call `std::basic_ios::init()` to complete initialization
   before first use or before destructor; otherwise the behavior is undefined.

### Notes

The same applies to the constructors of the next class in the I/O hierarchy,
`std::basic_ios`. Further-derived classes (`std::istream` and `std::ostream`)
are always constructed with a concrete stream buffer object and call
`std::basic_ios::init()`, possibly more than once, to complete initialization of
their virtual base.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 50 | C++98 | the copy constructor was not specified | specified as private
  LWG 220 | C++98 | the behavior of destroying a `std::ios_base` object before
      calling its `init()` was not clear | the behavior is undefined in this
      case
  LWG 1249 | C++98 | initialization did not need to be completed before first
      use | also needs to be completed

---
*Source: https://en.cppreference.com/w/cpp/io/ios_base/ios_base*
