# std::front_insert_iterator<Container>::front_insert_iterator

```cpp
explicit front_insert_iterator( Container& c );  // (until C++20)
constexpr explicit front_insert_iterator( Container& c );  // (since C++20)
```

Initializes the underlying pointer to the container to `std::addressof(c)`.

### Parameters

- **c** — container to initialize the inserter with

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2325R3 | C++20 | default constructor was provided as C++20 iterators must be
      `default_initializable` | removed along with the requirement

---
*Source: https://en.cppreference.com/w/cpp/iterator/front_insert_iterator/front_insert_iterator*
