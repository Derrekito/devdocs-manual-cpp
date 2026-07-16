# std::set<Key,Compare,Allocator>::set

```cpp
set();  // (until C++11)
set()
   : set(Compare()) {}  // (since C++11)
explicit set( const Compare& comp,
              const Allocator& alloc = Allocator() );  // (2)
explicit set( const Allocator& alloc );  // (3) (since C++11)
template< class InputIt >
set( InputIt first, InputIt last,
     const Compare& comp = Compare(),
     const Allocator& alloc = Allocator() );  // (4)
template< class InputIt >
set( InputIt first, InputIt last,
     const Allocator& alloc )
   : set(first, last, Compare(), alloc) {}  // (5) (since C++14)
set( const set& other );  // (6)
set( const set& other, const Allocator& alloc );  // (7) (since C++11)
set( set&& other );  // (8) (since C++11)
set( set&& other, const Allocator& alloc );  // (9) (since C++11)
set( std::initializer_list<value_type> init,
     const Compare& comp = Compare(),
     const Allocator& alloc = Allocator() );  // (10) (since C++11)
set( std::initializer_list<value_type> init,
     const Allocator& alloc )
   : set(init, Compare(), alloc) {}  // (11) (since C++14)
template< container-compatible-range<value_type> R >
set( std::from_range_t, R&& rg,
     const Compare& comp = Compare(),
     const Allocator& alloc = Allocator() );  // (12) (since C++23)
template< container-compatible-range<value_type> R >
set( std::from_range_t, R&& rg,
     const Allocator& alloc )
   : set(std::from_range, std::forward<R>(rg), Compare(), alloc) {}  // (13) (since C++23)
```

Constructs new container from a variety of data sources and optionally using
user supplied allocator `alloc` or comparison function object `comp`.

1-3) Constructs an empty container.

4,5) Constructs the container with the contents of the range
   `[``first``,``last``)`. If multiple elements in the range have keys that
   compare equivalent, it is unspecified which element is inserted (pending
   LWG2844).

If `[``first``,``last``)` is not a valid range, the behavior is undefined.

6,7) Copy constructor. Constructs the container with the copy of the contents of
   `other`. If `alloc` is not provided, allocator is obtained by calling
   `std::allocator_traits<allocator_type>::
   select_on_container_copy_construction(other.get_allocator())`. (since C++11)
   During class template argument deduction, only the first argument contributes
   to the deduction of the container's `Allocator` template parameter. (since
   C++23)

8,9) Move constructor. Constructs the container with the contents of `other`
   using move semantics. If `alloc` is not provided, allocator is obtained by
   move-construction from the allocator belonging to `other`. During class
   template argument deduction, only the first argument contributes to the
   deduction of the container's `Allocator` template parameter. (since C++23)

10,11) Initializer-list constructor. Constructs the container with the contents
   of the initializer list `init`.If multiple elements in the range have keys
   that compare equivalent, it is unspecified which element is inserted (pending
   LWG2844).

12,13) Constructs the container with the contents of `rg`. If multiple elements
   in the range have keys that compare equivalent, it is unspecified which
   element is inserted (pending LWG2844).

### Parameters

- **alloc** — allocator to use for all memory allocations of this container
- **comp** — comparison function object to use for all comparisons of keys
- **first, last** — the range to copy the elements from
- **other** — another container to be used as source to initialize the elements
  of the container with
- **init** — initializer list to initialize the elements of the container with
- **rg** — a container compatible range, that is, an `input_range` whose
  elements are convertible to `value_type`

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-`Compare` must meet the requirements of Compare.**

**-`Allocator` must meet the requirements of Allocator.**

### Complexity

1-3) Constant.

4,5) \(\scriptsize N \cdot log(N)\)N·log(N) where \(\scriptsize N\)N is
   `std::distance(first, last)` in general, linear in \(\scriptsize N\)N if
   `[``first``,``last``)` is already sorted by `value_comp()`.

6,7) Linear in size of `other`.

8,9) Constant. If `alloc` is given and `alloc != other.get_allocator()`, then
   linear.

10,11) \(\scriptsize N \cdot log(N)\)N·log(N) where \(\scriptsize N\)N is
   `init.size()` in general, linear in \(\scriptsize N\)N if `init` is already
   sorted by `value_comp()`.

12,13) \(\scriptsize N \cdot log(N)\)N·log(N) where \(\scriptsize N\)N is
   `ranges::distance(rg)` in general, linear in \(\scriptsize N\)N if `rg` is
   already sorted by `value_comp()`.

### Exceptions

Calls to `Allocator::allocate` may throw.

### Notes

After container move construction (overload (8,9)), references, pointers, and
iterators (other than the end iterator) to `other` remain valid, but refer to
elements that are now in `*this`. The current standard makes this guarantee via
the blanket statement in [container.reqmts]/67, and a more direct guarantee is
under consideration via LWG issue 2321.

Although not formally required until C++23, some implementations has already put
the template parameter `Allocator` into non-deduced contexts in earlier modes.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | Ranges-aware construction
      and insertion; overloads (12,13)

### Example

```cpp
#include <cmath>
#include <iostream>
#include <set>
#include <string>

struct Point { double x, y; };

struct PointCmp
{
    bool operator()(const Point& lhs, const Point& rhs) const
    {
        return std::hypot(lhs.x, lhs.y) < std::hypot(rhs.x, rhs.y);
    }
};

int main()
{
    // (1) Default constructor
    std::set<std::string> a;
    a.insert("cat");
    a.insert("dog");
    a.insert("horse");
    for (auto& str : a)
        std::cout << str << ' ';
    std::cout << '\n';

    // (4) Range constructor
    std::set<std::string> b(a.find("dog"), a.end());
    for (auto& str : b)
        std::cout << str << ' ';
    std::cout << '\n';

    // (6) Copy constructor
    std::set<std::string> c(a);
    c.insert("another horse");
    for (auto& str : c)
        std::cout << str << ' ';
    std::cout << '\n';

    // (8) Move constructor
    std::set<std::string> d(std::move(a));
    for (auto& str : d)
        std::cout << str << ' ';
    std::cout << '\n';
    std::cout << "moved-from set is ";
    for (auto& str : a)
        std::cout << str << ' ';
    std::cout << '\n';

    // (10) Initializer list constructor
    std::set<std::string> e{"one", "two", "three", "five", "eight"};
    for (auto& str : e)
        std::cout << str << ' ';
    std::cout << '\n';

    // Custom comparison
    std::set<Point, PointCmp> z = {{2, 5}, {3, 4}, {1, 1}};
    z.insert({1, -1}); // This fails because the magnitude of (1,-1) equals (1,1)
    for (auto& p : z)
        std::cout << '(' << p.x << ',' << p.y << ") ";
    std::cout << '\n';
}
```

Output:

```text
cat dog horse
dog horse
another horse cat dog horse
cat dog horse
moved-from set is
eight five one three two
(1,1) (3,4) (2,5)
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2076 | C++11 | overload (4) conditionally required `Key` to be
      CopyInsertable into `*this` | not required
  LWG 2193 | C++11 | the default constructor was explicit | made non-explicit

### See also

- **operator=** — assigns values to the container (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/set/set*
