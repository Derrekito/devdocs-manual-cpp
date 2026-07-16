# std::map<Key,T,Compare,Allocator>::operator[]

```cpp
T& operator[]( const Key& key );  // (1)
T& operator[]( Key&& key );  // (2) (since C++11)
```

Returns a reference to the value that is mapped to a key equivalent to `key`,
performing an insertion if such key does not already exist.

1) Inserts `value_type(key, T())` if the key does not exist.
**-`key_type` must meet the requirements of CopyConstructible.**
**-`mapped_type` must meet the requirements of CopyConstructible and DefaultConstructible.**
If an insertion is performed, the mapped value is value-initialized
(default-constructed for class types, zero-initialized otherwise) and a
reference to it is returned.
*(until C++11)*

1) Inserts a `value_type` object constructed in-place from
`std::piecewise_construct, std::forward_as_tuple(key), std::tuple<>()` if the
key does not exist. This function is equivalent to `return
this->try_emplace(key).first->second;`.(since C++17)When the default allocator
is used, this results in the key being copy constructed from `key` and the
mapped value being value-initialized.
**-`value_type` must be EmplaceConstructible from `std::piecewise_construct, std::forward_as_tuple(key), std::tuple<>()`. When the default allocator is used, this means that `key_type` must be CopyConstructible and `mapped_type` must be DefaultConstructible.**
2) Inserts a `value_type` object constructed in-place from
`std::piecewise_construct, std::forward_as_tuple(std::move(key)),
std::tuple<>()` if the key does not exist. This function is equivalent to
`return this->try_emplace(std::move(key)).first->second;`.(since C++17)
When the default allocator is used, this results in the key being move
constructed from `key` and the mapped value being value-initialized.
**-`value_type` must be EmplaceConstructible from `std::piecewise_construct, std::forward_as_tuple(std::move(key)), std::tuple<>()`. When the default allocator is used, this means that `key_type` must be MoveConstructible and `mapped_type` must be DefaultConstructible.**
*(since C++11)*

No iterators or references are invalidated.

### Parameters

- **key** — the key of the element to find

### Return value

Reference to the mapped value of the new element if no element with key `key`
existed. Otherwise a reference to the mapped value of the existing element whose
key is equivalent to `key`.

### Exceptions

If an exception is thrown by any operation, the insertion has no effect.

### Complexity

Logarithmic in the size of the container.

### Notes

In the published C++11 and C++14 standards, this function was specified to
require `mapped_type` to be DefaultInsertable and `key_type` to be
CopyInsertable or MoveInsertable into `*this`. This specification was defective
and was fixed by LWG issue 2469, and the description above incorporates the
resolution of that issue.

However, one implementation (libc++) is known to construct the `key_type` and
`mapped_type` objects via two separate allocator `construct()` calls, as
arguably required by the standards as published, rather than emplacing a
`value_type` object.

`operator[]` is non-const because it inserts the key if it doesn't exist. If
this behavior is undesirable or if the container is `const`, `at()` may be used.

`insert_or_assign()` returns more information than `operator[]` and does not
require default-constructibility of the mapped type.
*(since C++17)*

### Example

```cpp
#include <iostream>
#include <string>
#include <map>

auto print = [](auto const comment, auto const& map)
{
    std::cout << comment << '{';
    for (const auto& pair : map)
        std::cout << '{' << pair.first << ": " << pair.second << '}';
    std::cout << "}\n";
};

int main()
{
    std::map<char, int> letter_counts{{'a', 27}, {'b', 3}, {'c', 1}};

    print("letter_counts initially contains: ", letter_counts);

    letter_counts['b'] = 42; // updates an existing value
    letter_counts['x'] = 9;  // inserts a new value

    print("after modifications it contains: ", letter_counts);

    // count the number of occurrences of each word
    // (the first call to operator[] initialized the counter with zero)
    std::map<std::string, int>  word_map;
    for (const auto& w : {"this", "sentence", "is", "not", "a", "sentence",
                          "this", "sentence", "is", "a", "hoax"})
        ++word_map[w];
    word_map["that"]; // just inserts the pair {"that", 0}

    for (const auto& [word, count] : word_map)
        std::cout << count << " occurrence(s) of word '" << word << "'\n";
}
```

Output:

```text
letter_counts initially contains: {{a: 27}{b: 3}{c: 1}}
after modifications it contains: {{a: 27}{b: 42}{c: 1}{x: 9}}
2 occurrence(s) of word 'a'
1 occurrence(s) of word 'hoax'
2 occurrence(s) of word 'is'
1 occurrence(s) of word 'not'
3 occurrence(s) of word 'sentence'
0 occurrence(s) of word 'that'
2 occurrence(s) of word 'this'
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 334 | C++98 | the effect of overload (1) was simply returning
      `(*((insert(std::make_pair(x, T()))).first)).second` | provided its own
      description instead

### See also

- **at** — access specified element with bounds checking (public member
  function)
- **insert_or_assign (C++17)** — inserts an element or assigns to the current
  element if the key already exists (public member function)
- **try_emplace (C++17)** — inserts in-place if the key does not exist, does
  nothing if the key exists (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/map/operator_at*
