# std::forward_list<T,Allocator>::forward_list

```cpp
forward_list();  // (1) (since C++11)
explicit forward_list( const Allocator& alloc );  // (2) (since C++11)
forward_list( size_type count,
              const T& value,
              const Allocator& alloc = Allocator());  // (3) (since C++11)
explicit forward_list( size_type count );  // (since C++11) (until C++14)
explicit forward_list( size_type count,
                       const Allocator& alloc = Allocator() );  // (since C++14)
template< class InputIt >
forward_list( InputIt first, InputIt last,
              const Allocator& alloc = Allocator() );  // (5) (since C++11)
forward_list( const forward_list& other );  // (6) (since C++11)
forward_list( const forward_list& other, const Allocator& alloc );  // (7) (since C++11)
forward_list( forward_list&& other );  // (8) (since C++11)
forward_list( forward_list&& other, const Allocator& alloc );  // (9) (since C++11)
forward_list( std::initializer_list<T> init,
              const Allocator& alloc = Allocator() );  // (10) (since C++11)
template< container-compatible-range<T> R >
forward_list( std::from_range_t, R&& rg,
              const Allocator& alloc = Allocator() );  // (11) (since C++23)
```

Constructs a new container from a variety of data sources, optionally using a
user supplied allocator `alloc`.

1) Default constructor. Constructs an empty container with a default-constructed
   allocator.

2) Constructs an empty container with the given allocator `alloc`.

3) Constructs the container with `count` copies of elements with value `value`.

4) Constructs the container with `count` default-inserted instances of `T`. No
   copies are made.

5) Constructs the container with the contents of the range
   `[``first``,``last``)`. This overload participates in overload resolution
   only if `InputIt` satisfies LegacyInputIterator, to avoid ambiguity with the
   overload (3).

6) Copy constructor. Constructs the container with the copy of the contents of
   `other`. The allocator is obtained as if by calling
   `std::allocator_traits<allocator_type>::select_on_container_copy_construction(
   other.get_allocator())`.

7) Constructs the container with the copy of the contents of `other`, using
   `alloc` as the allocator. During class template argument deduction, only the
   first argument contributes to the deduction of the container's `Allocator`
   template parameter. (since C++23)

8) Move constructor. Constructs the container with the contents of `other` using
   move semantics. Allocator is obtained by move-construction from the allocator
   belonging to `other`.

9) Allocator-extended move constructor. Using `alloc` as the allocator for the
   new container, moving the contents from `other`; if `alloc !=
   other.get_allocator()`, this results in an element-wise move. During class
   template argument deduction, only the first argument contributes to the
   deduction of the container's `Allocator` template parameter. (since C++23)

10) Constructs the container with the contents of the initializer list `init`.

11) Constructs the container with the contents of the range `rg`.

### Parameters

- **alloc** — allocator to use for all memory allocations of this container
- **count** — the size of the container
- **value** — the value to initialize elements of the container with
- **first, last** — the range `[``first``,``last``)` to copy the elements from
- **other** — another container to be used as source to initialize the elements
  of the container with
- **init** — initializer list to initialize the elements of the container with
- **rg** — a container compatible range, that is, an `input_range` whose
  elements are convertible to `T`

### Complexity

1,2) Constant.

3,4) Linear in `count`.

5) Linear in distance between `first` and `last`.

6,7) Linear in size of `other`.

8) Constant.

9) Linear if `alloc != other.get_allocator()`, otherwise constant.

10) Linear in size of `init`.

11) Linear in `ranges::distance(rg)`.

### Exceptions

Calls to `Allocator::allocate` may throw.

### Notes

After container move construction (overload (8)), references, pointers, and
iterators (other than the end iterator) to `other` remain valid, but refer to
elements that are now in `*this`. The current standard makes this guarantee via
the blanket statement in [container.reqmts]/67, and a more direct guarantee is
under consideration via LWG issue 2321.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | Ranges-aware construction
      and insertion; overload (11)

### Example

```cpp
#include <iostream>
#include <string>
#include <forward_list>

template<typename T>
std::ostream& operator<<(std::ostream& s, const std::forward_list<T>& v)
{
    s.put('{');
    for (char comma[]{'\0', ' ', '\0'}; const auto& e : v)
        s << comma << e, comma[0] = ',';
    return s << "}\n";
}

int main()
{
    // C++11 initializer list syntax:
    std::forward_list<std::string> words1{"the", "frogurt", "is", "also", "cursed"};
    std::cout << "1: " << words1;

    // words2 == words1
    std::forward_list<std::string> words2(words1.begin(), words1.end());
    std::cout << "2: " << words2;

    // words3 == words1
    std::forward_list<std::string> words3(words1);
    std::cout << "3: " << words3;

    // words4 is {"Mo", "Mo", "Mo", "Mo", "Mo"}
    std::forward_list<std::string> words4(5, "Mo");
    std::cout << "4: " << words4;

    auto const rg = {"cat", "cow", "crow"};
#ifdef __cpp_lib_containers_ranges
    std::forward_list<std::string> words5(std::from_range, rg); // overload (11)
#else
    std::forward_list<std::string> words5(rg.begin(), rg.end()); // overload (5)
#endif
    std::cout << "5: " << words5;
}
```

Output:

```text
1: {the, frogurt, is, also, cursed}
2: {the, frogurt, is, also, cursed}
3: {the, frogurt, is, also, cursed}
4: {Mo, Mo, Mo, Mo, Mo}
5: {cat, cow, crow}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2193 | C++11 | the default constructor is explicit | made non-explicit

### See also

- **assign** — assigns values to the container (public member function)
- **operator=** — assigns values to the container (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/forward_list/forward_list*
