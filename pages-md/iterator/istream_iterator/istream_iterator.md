# std::istream_iterator<T,CharT,Traits,Distance>::istream_iterator

```cpp
istream_iterator();  // (until C++11)
constexpr istream_iterator();  // (since C++11)
constexpr istream_iterator( std::default_sentinel_t );  // (2) (since C++20)
istream_iterator( istream_type& stream );  // (3)
istream_iterator( const istream_iterator& other );  // (until C++11)
istream_iterator( const istream_iterator& other ) = default;  // (since C++11)
```

1,2) Constructs the end-of-stream iterator, value-initializes the stored value.
   This constructor is constexpr if the initializer in the definition `auto x =
   T();` is a constant initializer(since C++11).

3) Initializes the iterator, stores the address of `stream` in a data member,
   and performs the first read from the input stream to initialize the cached
   value data member.

4) Constructs a copy of `other`. If
   `std::is_trivially_copy_constructible<T>::value` is `true`, this copy
   constructor is a trivial copy constructor.(since C++11)

### Parameters

- **stream** — stream to initialize the `istream_iterator` with
- **other** — another `istream_iterator` of the same type

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <sstream>

int main()
{
    std::istringstream stream("1 2 3 4 5");
    std::copy(
        std::istream_iterator<int>(stream),
        std::istream_iterator<int>(),
        std::ostream_iterator<int>(std::cout, " ")
    );
}
```

Output:

```text
1 2 3 4 5
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 1280 | C++98 | overload (3) stored `stream` directly | stores its address
      instead
  P0738R2 | C++98 | the first read might be deferred to the first dereference |
      always performed in the constructor

---
*Source: https://en.cppreference.com/w/cpp/iterator/istream_iterator/istream_iterator*
