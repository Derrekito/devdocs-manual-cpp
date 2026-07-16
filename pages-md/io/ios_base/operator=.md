# std::ios_base::operator=

```cpp
private:
ios_base& operator=( const ios_base& );  // (until C++11)
public:
ios_base& operator=( const ios_base& ) = delete;  // (since C++11)
```

The copy assignment operator is private(until C++11)deleted(since C++11):
streams are not copy-assignable.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 50 | C++98 | the copy assignment operator was not specified | specified as
      private

---
*Source: https://en.cppreference.com/w/cpp/io/ios_base/operator%3D*
