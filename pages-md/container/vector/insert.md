# std::vector<T,Allocator>::insert

```cpp
iterator insert( const_iterator pos, const T& value );  // (until C++20)
constexpr iterator insert( const_iterator pos, const T& value );  // (since C++20)
iterator insert( const_iterator pos, T&& value );  // (since C++11) (until C++20)
constexpr iterator insert( const_iterator pos, T&& value );  // (since C++20)
iterator insert( const_iterator pos, size_type count, const T& value );  // (until C++20)
constexpr iterator
    insert( const_iterator pos, size_type count, const T& value );  // (since C++20)
template< class InputIt >
iterator insert( const_iterator pos, InputIt first, InputIt last );  // (until C++20)
template< class InputIt >
constexpr iterator insert( const_iterator pos, InputIt first, InputIt last );  // (since C++20)
iterator insert( const_iterator pos, std::initializer_list<T> ilist );  // (since C++11) (until C++20)
constexpr iterator insert( const_iterator pos,
                           std::initializer_list<T> ilist );  // (since C++20)
```

Inserts elements at the specified location in the container.

1,2) Inserts `value` before `pos`.

3) Inserts `count` copies of the `value` before `pos`.

4) Inserts elements from range `[``first``,``last``)` before `pos`. This
   overload has the same effect as overload (3) if `InputIt` is an integral
   type. (until C++11) This overload participates in overload resolution only if
   `InputIt` qualifies as LegacyInputIterator, to avoid ambiguity with the
   overload (3). (since C++11) The behavior is undefined if `first` and `last`
   are iterators into `*this`.

5) Inserts elements from initializer list `ilist` before `pos`.

If after the operation the new `size()` is greater than old `capacity()` a
reallocation takes place, in which case all iterators (including the `end()`
iterator) and all references to the elements are invalidated. Otherwise, only
the iterators and references before the insertion point remain valid.

### Parameters

- **pos** — iterator before which the content will be inserted (`pos` may be the
  `end()` iterator)
- **value** — element value to insert
- **count** — number of elements to insert
- **first, last** — the range of elements to insert, cannot be iterators into
  container for which insert is called
- **ilist** — initializer list to insert the values from

**Type requirements**

**-`T` must meet the requirements of CopyAssignable and CopyInsertable in order to use overload (1).**

**-`T` must meet the requirements of MoveAssignable and MoveInsertable in order to use overload (2).**

**-`T` must meet the requirements of CopyAssignable and CopyInsertable in order to use overload (3).**

**-`T` must meet the requirements of EmplaceConstructible in order to use overload (4,5).**

**-`T` must meet the requirements of MoveAssignable and MoveInsertable in order to use overload (4). required only if `InputIt` satisfies LegacyInputIterator but not LegacyForwardIterator. (until C++17)**

**-`T` must meet the requirements of Swappable, MoveAssignable, MoveConstructible and MoveInsertable in order to use overload (4,5). (since C++17)**

### Return value

1,2) Iterator pointing to the inserted `value`.

3) Iterator pointing to the first element inserted, or `pos` if `count == 0`.

4) Iterator pointing to the first element inserted, or `pos` if `first == last`.

5) Iterator pointing to the first element inserted, or `pos` if `ilist` is
   empty.

### Complexity

1,2) Constant plus linear in the distance between `pos` and end of the
   container.

3) Linear in `count` plus linear in the distance between `pos` and end of the
   container.

4) Linear in `std::distance(first, last)` plus linear in the distance between
   `pos` and end of the container.

5) Linear in `ilist.size()` plus linear in the distance between `pos` and end of
   the container.

### Exceptions

If an exception is thrown other than by

- the copy constructor of `T`,

- the move constructor of `T`,
*(since C++11)*

- the copy assignment operator of `T`,

- the move assignment operator of `T`,
*(since C++11)*

- any `InputIt` operation,

these functions have no effect (strong exception safety guarantee).

If an exception is thrown when inserting a single element at the end, and `T` is
CopyInsertable or `std::is_nothrow_move_constructible<T>::value` is `true`, this
function has no effect (strong exception guarantee). Otherwise, if an exception
is thrown by the move constructor of a non-CopyInsertable `T`, the effects are
unspecified.
*(since C++11)*

### Example

```cpp
#include <iostream>
#include <iterator>
#include <vector>

void print(int id, const std::vector<int>& container)
{
    std::cout << id << ". ";
    for (const int x : container)
        std::cout << x << ' ';
    std::cout << '\n';
}

int main ()
{
    std::vector<int> c1(3, 100);
    print(1, c1);

    auto it = c1.begin();
    it = c1.insert(it, 200);
    print(2, c1);

    c1.insert(it, 2, 300);
    print(3, c1);

    // `it` no longer valid, get a new one:
    it = c1.begin();

    std::vector<int> c2(2, 400);
    c1.insert(std::next(it, 2), c2.begin(), c2.end());
    print(4, c1);

    int arr[] = {501, 502, 503};
    c1.insert(c1.begin(), arr, arr + std::size(arr));
    print(5, c1);

    c1.insert(c1.end(), {601, 602, 603});
    print(6, c1);
}
```

Output:

```text
1. 100 100 100
2. 200 100 100 100
3. 300 300 200 100 100 100
4. 300 300 400 400 200 100 100 100
5. 501 502 503 300 300 400 400 200 100 100 100
6. 501 502 503 300 300 400 400 200 100 100 100 601 602 603
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 149 | C++98 | overloads (3) and (4) returned nothing | returns an iterator
  LWG 247 | C++98 | the complexity was only specified for overload (3) |
      specified for all overloads
  LWG 406 | C++98 | the strong exception guarantee also applied if the exception
      is thrown by an `InputIt` operation | no guarantee in this case

### See also

- **emplace (C++11)** — constructs element in-place (public member function)
- **push_back** — adds an element to the end (public member function)
- **inserter** — creates a `std::insert_iterator` of type inferred from the
  argument (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/insert*
