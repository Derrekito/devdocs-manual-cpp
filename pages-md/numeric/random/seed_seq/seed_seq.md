# std::seed_seq::seed_seq

```cpp
seed_seq() noexcept;  // (1) (since C++11)
seed_seq( const seed_seq& ) = delete;  // (2) (since C++11)
template< class InputIt >
seed_seq( InputIt begin, InputIt end );  // (3) (since C++11)
template< class T >
seed_seq( std::initializer_list<T> il );  // (4) (since C++11)
```

1) The default constructor creates a `std::seed_seq` object with an initial seed
   sequence of length zero.

2) The copy constructor is deleted: `std::seed_seq` is not copyable.

3) Constructs a `std::seed_seq` with the initial seed sequence obtained by
   iterating over the range `[``begin``,``end``)` and copying the values
   obtained by dereferencing the iterator, modulo 232 (that is, the lower 32
   bits are copied).

4) Equivalent to `seed_seq(il.begin(), il.end())`. This constructor enables
   list-initialization from the list of seed values. This overload participates
   in overload resolution only if `T` is an integer type.

### Parameters

- **begin, end** — the initial seed sequence represented as a pair of input
  iterators whose `std::iterator_traits<>::value_type` is an integer type
- **il** — `std::initializer_list` of objects of integer type, providing the
  initial seed sequence

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

### Exceptions

3,4) Throws `std::bad_alloc` on allocation failure.

### Example

```cpp
#include <iterator>
#include <random>
#include <sstream>

int main()
{
    std::seed_seq s1; // default-constructible
    std::seed_seq s2{1, 2, 3}; // can use list-initialization
    std::seed_seq s3 = {-1, 0, 1}; // another form of list-initialization
    int a[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    std::seed_seq s4(a, a + 10); // can use iterators
    std::istringstream buf("1 2 3 4 5");
    std::istream_iterator<int> beg(buf), end;
    std::seed_seq s5(beg, end); // even stream input iterators
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3422 | C++11 | the default constructor never fails but might be not
      noexcept; the initializer list constructor disabled list-initialization
      from iterator pairs | made noexcept; constrained

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/seed_seq/seed_seq*
