# operator==,!=,<,<=,>,>=,<=>(std::stack)

```cpp
template< class T, class Container >
bool operator==( const std::stack<T, Container>& lhs,
                 const std::stack<T, Container>& rhs );  // (1)
template< class T, class Container >
bool operator!=( const std::stack<T, Container>& lhs,
                 const std::stack<T, Container>& rhs );  // (2)
template< class T, class Container >
bool operator< ( const std::stack<T, Container>& lhs,
                 const std::stack<T, Container>& rhs );  // (3)
template< class T, class Container >
bool operator<=( const std::stack<T, Container>& lhs,
                 const std::stack<T, Container>& rhs );  // (4)
template< class T, class Container >
bool operator> ( const std::stack<T, Container>& lhs,
                 const std::stack<T, Container>& rhs );  // (5)
template< class T, class Container >
bool operator>=( const std::stack<T, Container>& lhs,
                 const std::stack<T, Container>& rhs );  // (6)
template< class T, std::three_way_comparable Container >
std::compare_three_way_result_t<Container>
    operator<=>( const std::stack<T, Container>& lhs,
                 const std::stack<T, Container>& rhs );  // (7) (since C++20)
```

Compares the contents of the underlying containers of two container adaptors.
The comparison is done by applying the corresponding operator to the underlying
containers.

### Parameters

- **lhs, rhs** — container adaptors whose contents to compare

**-`T` must meet the requirements of EqualityComparable.**

### Return value

1-6) `true` if the corresponding comparison yields `true`, `false` otherwise.

7) Result of three-way comparison on underlying containers.

### Complexity

Linear in the size of the container.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 410 | C++98 | the semantics of all comparison operators were missing |
      added

---
*Source: https://en.cppreference.com/w/cpp/container/stack/operator_cmp*
