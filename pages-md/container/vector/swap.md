# std::vector<T,Allocator>::swap

```cpp
void swap( vector& other );  // (until C++17)
void swap( vector& other ) noexcept(/* see below */);  // (since C++17) (until C++20)
constexpr void swap( vector& other ) noexcept(/* see below */);  // (since C++20)
```

Exchanges the contents and capacity of the container with those of `other`. Does
not invoke any move, copy, or swap operations on individual elements.

All iterators and references remain valid. The `end()` iterator is invalidated.

If `std::allocator_traits<allocator_type>::propagate_on_container_swap::value`
is `true`, then the allocators are exchanged using an unqualified call to
non-member `swap`. Otherwise, they are not swapped (and if `get_allocator() !=
other.get_allocator()`, the behavior is undefined).
*(since C++11)*

### Parameters

- **other** — container to exchange the contents with

### Return value

(none)

### Exceptions

(none)
*(until C++17)*

`noexcept` specification:
`noexcept(std::allocator_traits<Allocator>::propagate_on_container_swap::value
|| std::allocator_traits<Allocator>::is_always_equal::value)`
*(since C++17)*

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <vector>

template<class Os, class Co>
Os& operator<<(Os& os, const Co& co)
{
    os << '{';
    for (auto const& i : co)
        os << ' ' << i;
    return os << " } ";
}

int main()
{
    std::vector<int> a1{1, 2, 3}, a2{4, 5};

    auto it1 = std::next(a1.begin());
    auto it2 = std::next(a2.begin());

    int& ref1 = a1.front();
    int& ref2 = a2.front();

    std::cout << a1 << a2 << *it1 << ' ' << *it2 << ' ' << ref1 << ' ' << ref2 << '\n';
    a1.swap(a2);
    std::cout << a1 << a2 << *it1 << ' ' << *it2 << ' ' << ref1 << ' ' << ref2 << '\n';

    // Note that after swap the iterators and references stay associated with their
    // original elements, e.g. it1 that pointed to an element in 'a1' with value 2
    // still points to the same element, though this element was moved into 'a2'.
}
```

Output:

```text
{ 1 2 3 } { 4 5 } 2 5 1 4
{ 4 5 } { 1 2 3 } 2 5 1 4
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 341 | C++98 | the capacities of `std::vector` could not be swapped | they
      are also swapped

### See also

- **std::swap(std::vector)** — specializes the `std::swap` algorithm (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/swap*
