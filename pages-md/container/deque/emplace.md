# std::deque<T,Allocator>::emplace

```cpp
template< class... Args >
iterator emplace( const_iterator pos, Args&&... args );  // (since C++11)
```

Inserts a new element into the container directly before `pos`.

The element is constructed through `std::allocator_traits::construct`, which
typically uses placement-new to construct the element in-place at a location
provided by the container. However, if the required location has been occupied
by an existing element, the inserted element is constructed at another location
at first, and then move assigned into the required location.

The arguments `args...` are forwarded to the constructor as
`std::forward<Args>(args)...`. `args...` may directly or indirectly refer to a
value in the container.

All iterators (including the `end()` iterator) are invalidated. References are
invalidated too, unless `pos == begin()` or `pos == end()`, in which case they
are not invalidated.

### Parameters

- **pos** — iterator before which the new element will be constructed
- **args** — arguments to forward to the constructor of the element

**Type requirements**

**-`T (the container's element type)` must meet the requirements of MoveAssignable, MoveInsertable and EmplaceConstructible.**

### Return value

Iterator pointing to the emplaced element.

### Complexity

Linear in the lesser of the distances between `pos` and either of the ends of
the container.

### Exceptions

If an exception is thrown other than by the copy constructor, move constructor,
assignment operator, or move assignment operator of the value type, or if an
exception is thrown while `emplace` is used to insert a single element at the
either end, there are no effects (strong exception guarantee).

Otherwise, the effects are unspecified.

### Example

```cpp
#include <iostream>
#include <string>
#include <deque>

struct A
{
    std::string s;

    A(std::string str) : s(std::move(str)) { std::cout << " constructed\n"; }

    A(const A& o) : s(o.s) { std::cout << " copy constructed\n"; }

    A(A&& o) : s(std::move(o.s)) { std::cout << " move constructed\n"; }

    A& operator=(const A& other)
    {
        s = other.s;
        std::cout << " copy assigned\n";
        return *this;
    }

    A& operator=(A&& other)
    {
        s = std::move(other.s);
        std::cout << " move assigned\n";
        return *this;
    }
};

int main()
{
    std::deque<A> container;

    std::cout << "construct 2 times A:\n";
    A two { "two" };
    A three { "three" };

    std::cout << "emplace:\n";
    container.emplace(container.end(), "one");

    std::cout << "emplace with A&:\n";
    container.emplace(container.end(), two);

    std::cout << "emplace with A&&:\n";
    container.emplace(container.end(), std::move(three));

    std::cout << "content:\n";
    for (const auto& obj : container)
        std::cout << ' ' << obj.s;
    std::cout << '\n';
}
```

Output:

```text
construct 2 times A:
 constructed
 constructed
emplace:
 constructed
emplace with A&:
 copy constructed
emplace with A&&:
 move constructed
content:
 one two three
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2164 | C++11 | it was not clear whether the arguments can refer to the
      container | clarified

### See also

- **insert** — inserts elements (public member function)
- **emplace_back (C++11)** — constructs an element in-place at the end (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/container/deque/emplace*
