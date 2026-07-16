# C++ named requirements: Container

A **Container** is an object used to store other objects and taking care of the
management of the memory used by the objects it contains.

### Requirements

- `T`, an element type;
- `C`, a Container type containing elements of type `T`;
- `a` and `b`, objects of type `C`;
- `rv`, a prvalue expression of type `C`.

#### Types

  Name | Type | Requirements
  `value_type` | `T` | CopyConstructible(until C++11)Erasable(since C++11)
  `reference` | `T&`
  `const_reference` | const T&
  `iterator` | Iterator whose value type is `T` | LegacyForwardIterator
      convertible to `const_iterator`
  `const_iterator` | Constant iterator whose value type is `T` |
      LegacyForwardIterator
  `difference_type` | Signed integer | Must be the same as
      `iterator_traits::difference_type` for `iterator` and `const_iterator`
  `size_type` | Unsigned integer | Large enough to represent all positive values
      of `difference_type`

#### Member functions and operators

  Expression | Return type | Semantics | Conditions | Complexity
  `C()` | `C` | Creates an empty container | Post: `C().empty()` `== true` |
      Constant
  `C(a)` | `C` | Creates a copy of `a` | Pre: T must be CopyInsertable Post: `a
      == C(a)` | Linear
  `C(rv)` (since C++11) | `C` | Moves `rv` | Post: equal to the value `rv` had
      before this construction | Constant[1]
  `a = b` | `C&` | Destroys or copy-assigns all elements of `a` from elements of
      `b` | Post: `a == b` | Linear
  `a = rv` (since C++11) | `C&` | Destroys or move-assigns all elements of `a`
      from elements of `rv` | Post: if `a` and `rv` do not refer the same
      object, `a` is equal to the value `rv` had before this assignment | Linear
  `a.~C()` | void | Destroys all elements of `a` and frees all memory | Linear
  `a.begin()` | `(const_)iterator` | Iterator to the first element of `a` |
      Constant
  `a.end()` | `(const_)iterator` | Iterator to one past the last element of `a`
      | Constant
  `a.cbegin()` (since C++11) | `const_iterator` | `const_cast<const
      C&>(a).begin()` | Constant
  `a.cend()` (since C++11) | `const_iterator` | `const_cast<const C&>(a).end()`
      | Constant
  `a == b` | Convertible to bool | `a.size() == b.size() &&
      std::equal(a.begin(), a. end(), b.begin())` (until C++14)
      `std::equal(a.begin(), a.end(), b.begin(), b.end())` (since C++14) |
      `a.size() == b.size() && std::equal(a.begin(), a. end(), b.begin())` |
      (until C++14) | `std::equal(a.begin(), a.end(), b.begin(), b.end())` |
      (since C++14) | Pre: `T` must be EqualityComparable | Constant[2] if
      `a.size() !=` `b.size()`, linear otherwise
  `a.size() == b.size() && std::equal(a.begin(), a. end(), b.begin())` | (until
      C++14)
  `std::equal(a.begin(), a.end(), b.begin(), b.end())` | (since C++14)
  `a != b` | convertible to bool | `!(a == b)` | Linear
  `a.swap(b)` | void | Exchanges the values of `a` and `b` | Constant[1][3]
  `swap(a, b)` | void | `a.swap(b)` | Constant[1]
  `a.size()` | `size_type` | `std::distance(a.begin(), a.end())` | Constant[3]
  `a.max_size()` | `size_type` | `b.size()` where `b` is the largest possible
      container | Constant[3]
  `a.empty()` | Convertible to bool | `a.begin() == a.end()` | Constant
  Notes
  (since C++11) Linear for `std::array` Always linear for `std::forward_list`
      (until C++11) Not strictly constant

Given

- `i` and `j`, objects of a container's `iterator` type,

in the expressions `i == j`, `i != j`, `i < j`, `i <= j`, `i >= j`, `i > j`, `i
- j`, either or both may be replaced by an object of the container's
`const_iterator` type referring to the same element with no change in semantics.

### Container data races

see container thread safety

### Other requirements

`C` (Container)

- DefaultConstructible
- CopyConstructible
- EqualityComparable
- Swappable

`T` (Type)

- CopyInsertable
- EqualityComparable
- Destructible

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 179 | C++98 | `iterator` and `const_iterator` types might be incomparable
      | required to be comparable
  LWG 276 | C++98 | `T` was required to be CopyAssignable | `T` is required to
      be CopyConstructible
  LWG 322 | C++98 | the value types of `iterator` and `const_iterator` were not
      specified | specified as `T`
  LWG 774 | C++98 | there was no requirement on `swap(a, b)` | added
  LWG 883 | C++98 | `a.swap(b)` was defined as `swap(a, b)`, resulted in
      circular definition | defined as exchanging the values of `a` and `b`
  LWG 1319 | C++98 | `iterator` and `const_iterator` might not have multipass
      guarantee | they are required to satisfy the requirements of
      LegacyForwardIterator
  LWG 2263 | C++11 | the resolution of LWG issue 179 was accidentally dropped in
      C++11 | restored
  LWG 2839 | C++11 | self move assignment of standard containers was not allowed
      | allowed but the result is unspecified

### See also

**C++ documentation for `Containers library`**

---
*Source: https://en.cppreference.com/w/cpp/named_req/Container*
