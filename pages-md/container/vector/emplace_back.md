# std::vector<T,Allocator>::emplace_back

```cpp
template< class... Args >
void emplace_back( Args&&... args );  // (since C++11) (until C++17)
template< class... Args >
reference emplace_back( Args&&... args );  // (since C++17) (until C++20)
template< class... Args >
constexpr reference emplace_back( Args&&... args );  // (since C++20)
```

Appends a new element to the end of the container. The element is constructed
through `std::allocator_traits::construct`, which typically uses placement-new
to construct the element in-place at the location provided by the container. The
arguments `args...` are forwarded to the constructor as
`std::forward<Args>(args)...`.

If after the operation the new `size()` is greater than old `capacity()` a
reallocation takes place, in which case all iterators (including the `end()`
iterator) and all references to the elements are invalidated. Otherwise only the
`end()` iterator is invalidated.

### Parameters

- **args** — arguments to forward to the constructor of the element

**Type requirements**

**-`T (the container's element type)` must meet the requirements of MoveInsertable and EmplaceConstructible.**

### Return value

(none)
*(until C++17)*

A reference to the inserted element.
*(since C++17)*

### Complexity

Amortized constant.

### Exceptions

If an exception is thrown, this function has no effect (strong exception
guarantee). If `T`'s move constructor is not `noexcept` and is not
CopyInsertable into `*this`, vector will use the throwing move constructor. If
it throws, the guarantee is waived and the effects are unspecified.

### Notes

Since reallocation may take place, `emplace_back` requires the element type to
be MoveInsertable for vectors.

### Example

The following code uses `emplace_back` to append an object of type `President`
to a `std::vector`. It demonstrates how `emplace_back` forwards parameters to
the `President` constructor and shows how using `emplace_back` avoids the extra
copy or move operation required when using `push_back`.

```cpp
#include <vector>
#include <cassert>
#include <iostream>
#include <string>

struct President
{
    std::string name;
    std::string country;
    int year;

    President(std::string p_name, std::string p_country, int p_year)
        : name(std::move(p_name)), country(std::move(p_country)), year(p_year)
    {
        std::cout << "I am being constructed.\n";
    }

    President(President&& other)
        : name(std::move(other.name)), country(std::move(other.country)), year(other.year)
    {
        std::cout << "I am being moved.\n";
    }

    President& operator=(const President& other) = default;
};

int main()
{
    std::vector<President> elections;
    std::cout << "emplace_back:\n";
    auto& ref = elections.emplace_back("Nelson Mandela", "South Africa", 1994);
    assert(ref.year == 1994 && "uses a reference to the created object (C++17)");

    std::vector<President> reElections;
    std::cout << "\npush_back:\n";
    reElections.push_back(President("Franklin Delano Roosevelt", "the USA", 1936));

    std::cout << "\nContents:\n";
    for (President const& president: elections)
        std::cout << president.name << " was elected president of "
                  << president.country << " in " << president.year << ".\n";

    for (President const& president: reElections)
        std::cout << president.name << " was re-elected president of "
                  << president.country << " in " << president.year << ".\n";
}
```

Output:

```text
emplace_back:
I am being constructed.

push_back:
I am being constructed.
I am being moved.

Contents:
Nelson Mandela was elected president of South Africa in 1994.
Franklin Delano Roosevelt was re-elected president of the USA in 1936.
```

### See also

- **push_back** — adds an element to the end (public member function)
- **emplace (C++11)** — constructs element in-place (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/emplace_back*
