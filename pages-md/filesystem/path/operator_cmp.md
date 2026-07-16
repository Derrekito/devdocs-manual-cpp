# operator==,!=,<,<=,>,>=,<=>(std::filesystem::path)

```cpp
friend bool operator==( const path& lhs, const path& rhs ) noexcept;  // (1) (since C++17)
friend bool operator!=( const path& lhs, const path& rhs ) noexcept;  // (2) (since C++17) (until C++20)
friend bool operator<( const path& lhs, const path& rhs ) noexcept;  // (3) (since C++17) (until C++20)
friend bool operator<=( const path& lhs, const path& rhs ) noexcept;  // (4) (since C++17) (until C++20)
friend bool operator>( const path& lhs, const path& rhs ) noexcept;  // (5) (since C++17) (until C++20)
friend bool operator>=( const path& lhs, const path& rhs ) noexcept;  // (6) (since C++17) (until C++20)
friend std::strong_ordering
    operator<=>( const path& lhs, const path& rhs ) noexcept;  // (7) (since C++20)
```

Compares two paths lexicographically.

1) Checks whether `lhs` and `rhs` are equal. Equivalent to `!(lhs < rhs) &&
   !(rhs < lhs)`.

2) Checks whether `lhs` and `rhs` are not equal. Equivalent to `!(lhs == rhs)`.

3) Checks whether `lhs` is less than `rhs`. Equivalent to `lhs.compare(rhs) <
   0`.

4) Checks whether `lhs` is less than or equal to `rhs`. Equivalent to `!(rhs <
   lhs)`.

5) Checks whether `lhs` is greater than `rhs`. Equivalent to `rhs < lhs`.

6) Checks whether `lhs` is greater than or equal to `rhs`. Equivalent to `!(lhs
   < rhs)`.

7) Obtains the three-way comparison result of `lhs` and `rhs`. Equivalent to
   `lhs.compare(rhs) <=> 0`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `std::filesystem::path` is
an associated class of the arguments. This prevents undesirable conversions in
the presence of a `using namespace std::filesystem;` *using-directive*.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.
*(since C++20)*

### Parameters

- **lhs, rhs** — the paths to compare

### Return value

1-6) `true` if the corresponding comparison yields, `false` otherwise.

7) `std::strong_ordering::less` if `lhs` is less than `rhs`, otherwise
   `std::strong_ordering::greater` if `rhs` is less than `lhs`, otherwise
   `std::strong_ordering::equal`.

### Notes

Path equality and equivalence have different semantics.

In the case of equality, as determined by `operator==`, only lexical
representations are compared. Therefore, `path("a") == path("b")` is never
`true`.

In the case of equivalence, as determined by `std::filesystem::equivalent()`, it
is checked whether two paths *resolve* to the same file system object. Thus
`equivalent("a", "b")` will return `true` if the paths resolve to the same file.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3065 | C++17 | allowed comparison of everything convertible to `path` in
      the presence of a *using-directive* | made hidden friend

### See also

- **compare** — compares the lexical representations of two paths
  lexicographically (public member function)
- **equivalent (C++17)** — checks whether two paths refer to the same file
  system object (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/operator_cmp*
