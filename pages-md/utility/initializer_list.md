# std::initializer_list

(not to be confused with member initializer list)

```cpp
template< class T >
class initializer_list;  // (since C++11)
```

An object of type `std::initializer_list<T>` is a lightweight proxy object that
provides access to an array of objects of type const T (that may be allocated in
read-only memory).

A `std::initializer_list` object is automatically constructed when:

- a braced-init-list is used to list-initialize an object, where the
  corresponding constructor accepts an `std::initializer_list` parameter,
- a braced-init-list is used as the right operand of assignment or as a function
  call argument, and the corresponding assignment operator/function accepts an
  `std::initializer_list` parameter,
- a braced-init-list is bound to `auto`, including in a ranged for loop.

`std::initializer_list` may be implemented as a pair of pointers or pointer and
length. Copying a `std::initializer_list` does not copy the backing array of the
corresponding initializer list.

The program is ill-formed if an explicit or partial specialization of
`std::initializer_list` is declared.

### Member types

- **`value_type`** — `T`
- **`reference`** — const T&
- **`const_reference`** — const T&
- **`size_type`** — `std::size_t`
- **`iterator`** — const T*
- **`const_iterator`** — const T*

### Member functions

- **(constructor)** — creates an empty initializer list (public member function)

**Capacity**

- **size** — returns the number of elements in the initializer list (public
  member function)

**Iterators**

- **begin** — returns a pointer to the first element (public member function)
- **end** — returns a pointer to one past the last element (public member
  function)

### Non-member functions

- **std::begin(std::initializer_list) (C++11)** — overloads `std::begin`
  (function template)
- **std::end(std::initializer_list) (C++11)** — specializes `std::end` (function
  template)

**Free function templates overloaded for `std::initializer_list`**

- **rbegincrbegin (C++14)** — returns a reverse iterator to the beginning of a
  container or array (function template)
- **rendcrend (C++14)** — returns a reverse end iterator for a container or
  array (function template)
- **empty (C++17)** — checks whether the container is empty (function template)
- **data (C++17)** — obtains the pointer to the underlying array (function
  template)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_initializer_lists` | 200806L | (C++11) | List-initialization and
      `std::initializer_list`

### Example

```cpp
#include <initializer_list>
#include <iostream>
#include <vector>

template<class T>
struct S
{
    std::vector<T> v;

    S(std::initializer_list<T> l) : v(l)
    {
         std::cout << "constructed with a " << l.size() << "-element list\n";
    }

    void append(std::initializer_list<T> l)
    {
        v.insert(v.end(), l.begin(), l.end());
    }

    std::pair<const T*, std::size_t> c_arr() const
    {
        return {&v[0], v.size()}; // copy list-initialization in return statement
                                  // this is NOT a use of std::initializer_list
    }
};

template<typename T>
void templated_fn(T) {}

int main()
{
    S<int> s = {1, 2, 3, 4, 5}; // copy list-initialization
    s.append({6, 7, 8});        // list-initialization in function call

    std::cout << "The vector now has " << s.c_arr().second << " ints:\n";

    for (auto n : s.v)
        std::cout << n << ' ';
    std::cout << '\n';

    std::cout << "Range-for over brace-init-list: \n";

    for (int x : {-1, -2, -3}) // the rule for auto makes this ranged-for work
        std::cout << x << ' ';
    std::cout << '\n';

    auto al = {10, 11, 12}; // special rule for auto

    std::cout << "The list bound to auto has size() = " << al.size() << '\n';

//  templated_fn({1, 2, 3}); // compiler error! "{1, 2, 3}" is not an expression,
                             // it has no type, and so T cannot be deduced
    templated_fn<std::initializer_list<int>>({1, 2, 3}); // OK
    templated_fn<std::vector<int>>({1, 2, 3});           // also OK
}
```

Output:

```text
constructed with a 5-element list
The vector now has 8 ints:
1 2 3 4 5 6 7 8
Range-for over brace-init-list:
-1 -2 -3
The list bound to auto has size() = 3
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2129 | C++11 | `std::initializer_list` could have explicit specializations
      or partial specializations | the program is ill-formed in this case

### See also

- **span (C++20)** — a non-owning view over a contiguous sequence of objects
  (class template)
- **basic_string_view (C++17)** — read-only string view (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/initializer_list*
