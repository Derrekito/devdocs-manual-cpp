# std::insert_iterator<Container>::insert_iterator

```cpp
insert_iterator( Container& c, typename Container::iterator i );  // (until C++20)
constexpr insert_iterator( Container& c, ranges::iterator_t<Container> i );  // (since C++20)
```

Initializes the underlying pointer to the container to `std::addressof(c)` and
the underlying iterator to `i`.

### Parameters

- **c** — container to initialize the inserter with
- **i** — iterator to initialize the inserter with

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 561 | C++98 | the type of `i` was independent of `Container` | it is the
      iterator type of `Container`
  P2325R3 | C++20 | default constructor was provided as C++20 iterators must be
      `default_initializable` | removed along with the requirement

---
*Source: https://en.cppreference.com/w/cpp/iterator/insert_iterator/insert_iterator*
