# std::match_results<BidirIt,Alloc>::operator=

```cpp
match_results& operator=( const match_results& other );  // (1) (since C++11)
match_results& operator=( match_results&& other ) noexcept;  // (2) (since C++11)
```

Assigns the contents.

1) Copy assignment operator. Assigns the contents of `other`.

2) Move assignment operator. Assigns the contents of `other` using move
   semantics. `other` is in a valid, but unspecified state after the operation.

### Parameters

- **other** — another match results object

### Return value

`*this`

### Exceptions

1) May throw implementation-defined exceptions.

---
*Source: https://en.cppreference.com/w/cpp/regex/match_results/operator%3D*
