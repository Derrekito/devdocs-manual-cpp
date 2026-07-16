# std::list<T,Allocator>::assign

```cpp
void assign( size_type count, const T& value );  // (1)
template< class InputIt >
void assign( InputIt first, InputIt last );  // (2)
void assign( std::initializer_list<T> ilist );  // (3) (since C++11)
```

Replaces the contents of the container.

1) Replaces the contents with `count` copies of value `value`.

2) Replaces the contents with copies of those in the range
   `[``first``,``last``)`. The behavior is undefined if either argument is an
   iterator into `*this`. This overload has the same effect as overload (1) if
   `InputIt` is an integral type. (until C++11) This overload participates in
   overload resolution only if `InputIt` satisfies LegacyInputIterator. (since
   C++11)

3) Replaces the contents with the elements from the initializer list `ilist`.

All iterators, pointers and references to the elements of the container are
invalidated.

### Parameters

- **count** — the new size of the container
- **value** — the value to initialize elements of the container with
- **first, last** — the range to copy the elements from
- **ilist** — initializer list to copy the values from

### Complexity

1) Linear in `count`.

2) Linear in distance between `first` and `last`.

3) Linear in `ilist.size()`.

### Example

The following code uses `assign` to add several characters to a std::list<char>:

```cpp
#include <list>
#include <iostream>
#include <string>

int main()
{
    std::list<char> characters;

    auto print_list = [&]()
    {
        for (char c : characters)
            std::cout << c << ' ';
        std::cout << '\n';
    };

    characters.assign(5, 'a');
    print_list();

    const std::string extra(6, 'b');
    characters.assign(extra.begin(), extra.end());
    print_list();

    characters.assign({'C', '+', '+', '1', '1'});
    print_list();
}
```

Output:

```text
a a a a a
b b b b b b
C + + 1 1
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 320 | C++98 | the replacement operation was defined as erasing all
      existing elements followed by inserting the given elements | removed the
      definition

### See also

- **(constructor)** — constructs the `list` (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/list/assign*
