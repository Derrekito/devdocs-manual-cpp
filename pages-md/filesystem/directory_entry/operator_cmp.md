# std::filesystem::directory_entry::operator==,!=,<,<=,>,>=,<=>

```cpp
bool operator==( const directory_entry& rhs ) const noexcept;  // (1) (since C++17)
bool operator!=( const directory_entry& rhs ) const noexcept;  // (2) (since C++17) (until C++20)
bool operator<( const directory_entry& rhs ) const noexcept;  // (3) (since C++17) (until C++20)
bool operator<=( const directory_entry& rhs ) const noexcept;  // (4) (since C++17) (until C++20)
bool operator>( const directory_entry& rhs ) const noexcept;  // (5) (since C++17) (until C++20)
bool operator>=( const directory_entry& rhs ) const noexcept;  // (6) (since C++17) (until C++20)
std::strong_ordering operator<=>( const directory_entry& rhs ) const noexcept;  // (7) (since C++20)
```

Compares the path with the directory entry `rhs`.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.
*(since C++20)*

### Parameters

- **rhs** — directory_entry to compare

### Return value

1) `true` if `path() == rhs.path()`, `false` otherwise.

2) `true` if `path() != rhs.path()`, `false` otherwise.

3) `true` if `path() < rhs.path()`, `false` otherwise.

4) `true` if `path() <= rhs.path()`, `false` otherwise.

5) `true` if `path() > rhs.path()`, `false` otherwise.

6) `true` if `path() >= rhs.path()`, `false` otherwise.

7) The result of `path() <=> rhs.path()`.

### See also

- **pathoperator const path&** — returns the path the entry refers to (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/directory_entry/operator_cmp*
