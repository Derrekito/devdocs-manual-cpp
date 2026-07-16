# operator==,!=,<,<=>(std::error_condition)

```cpp
bool operator==( const std::error_condition& lhs,
                 const std::error_condition& rhs ) noexcept;  // (1) (since C++11)
bool operator!=( const std::error_condition& lhs,
                 const std::error_condition& rhs ) noexcept;  // (2) (since C++11) (until C++20)
bool operator<( const std::error_condition& lhs,
                const std::error_condition& rhs ) noexcept;  // (3) (since C++11) (until C++20)
std::strong_ordering operator<=>( const std::error_condition& lhs,
                                  const std::error_condition& rhs ) noexcept;  // (4) (since C++20)
bool operator==( const std::error_code& code,
                 const std::error_condition& cond ) noexcept;  // (5) (since C++11)
bool operator==( const std::error_condition& cond,
                 const std::error_code& code ) noexcept;  // (5) (since C++11) (until C++20)
bool operator!=( const std::error_code& code,
                 const std::error_condition& cond ) noexcept;  // (6) (since C++11) (until C++20)
bool operator!=( const std::error_condition& cond,
                 const std::error_code& code ) noexcept;  // (6) (since C++11) (until C++20)
```

Compares two error conditions.

1) Checks whether `lhs` and `rhs` are equal.

2) Checks whether `lhs` and `rhs` are not equal.

3) Checks whether `lhs` is *less than* `rhs`.

4) Obtains three-way comparison result of `lhs` and `rhs`.

5) Checks whether `code` is a semantic match for `cond`.

6) Checks whether `code` is not a semantic match for `cond`.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.
*(since C++20)*

### Parameters

- **lhs, rhs, cond** — error conditions to compare
- **code** — the error code to compare

### Return value

1) `true` if the error category and error value compare equal.

2) `true` if the error category or error value compare are not equal.

3) `true` if `lhs.category() < rhs.category()`. Otherwise, `true` if
   `lhs.category() == rhs.category() && lhs.value() < rhs.value()`. Otherwise,
   `false`.

4) `lhs.category() <=> rhs.category()` if it is not
   `std::strong_ordering::equal`. Otherwise, `lhs.value() <=> rhs.value()`.

5) `true` if either `code.category().equivalent(code.value(), cond)` or
   `cond.category().equivalent(code, cond.value())`.

6) `true` if neither `code.category().equivalent(code.value(), cond)` nor
   `cond.category().equivalent(code, cond.value())`.

### See also

- **equivalent [virtual]** — compares `error_code` and `error_condition` for
  equivalence (virtual public member function of `std::error_category`)
- **operator==operator!=operator<operator<=> (removed in C++20)(removed in
  C++20)(C++20)** — compares two `error_code`s (function)

---
*Source: https://en.cppreference.com/w/cpp/error/error_condition/operator_cmp*
