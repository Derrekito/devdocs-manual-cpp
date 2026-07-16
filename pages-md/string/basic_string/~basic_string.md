# std::basic_string<CharT,Traits,Allocator>::~basic_string

```cpp
~basic_string();  // (until C++20)
constexpr ~basic_string();  // (since C++20)
```

Destructs the `basic_string`. The destructors of the elements are called and the
used storage is deallocated.

### Complexity

Typically constant (formally linear).

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/%7Ebasic_string*
