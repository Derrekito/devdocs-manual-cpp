# std::list<T,Allocator>::emplace

```cpp
template< class... Args >
iterator emplace( const_iterator pos, Args&&... args );  // (since C++11)
```

Inserts a new element into the container directly before `pos`.

The element is constructed through `std::allocator_traits::construct`, which
uses placement-new to construct the element in-place at a location provided by
the container.

The arguments `args...` are forwarded to the constructor as
`std::forward<Args>(args)...`. `args...` may directly or indirectly refer to a
value in the container.

No iterators or references are invalidated.

### Parameters

- **pos** — iterator before which the new element will be constructed
- **args** — arguments to forward to the constructor of the element

**Type requirements**

**-`T (the container's element type)` must meet the requirements of EmplaceConstructible.**

### Return value

Iterator pointing to the emplaced element.

### Complexity

Constant.

### Exceptions

If an exception is thrown (e.g. by the constructor), the container is left
unmodified, as if this function was never called (strong exception guarantee).

### Example

```cpp
#include <iostream>
#include <string>
#include <list>

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
    std::list<A> container;

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
*Source: https://en.cppreference.com/w/cpp/container/list/emplace*
