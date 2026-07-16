# operator==, !=, <, <=, >, >=, <=>(std::variant)

```cpp
template< class... Types >
constexpr bool operator==( const std::variant<Types...>& v,
                           const std::variant<Types...>& w );  // (1) (since C++17)
template< class... Types >
constexpr bool operator!=( const std::variant<Types...>& v,
                           const std::variant<Types...>& w );  // (2) (since C++17)
template< class... Types >
constexpr bool operator<( const std::variant<Types...>& v,
                          const std::variant<Types...>& w );  // (3) (since C++17)
template< class... Types >
constexpr bool operator>( const std::variant<Types...>& v,
                          const std::variant<Types...>& w );  // (4) (since C++17)
template< class... Types >
constexpr bool operator<=( const std::variant<Types...>& v,
                           const std::variant<Types...>& w );  // (5) (since C++17)
template< class... Types >
constexpr bool operator>=( const std::variant<Types...>& v,
                           const std::variant<Types...>& w );  // (6) (since C++17)
template< class... Types >
constexpr std::common_comparison_category_t<
    std::compare_three_way_result_t<Types>...>
    operator<=>( const std::variant<Types...>& v,
                 const std::variant<Types...>& w );  // (7) (since C++20)
```

1) Equality operator for variants:

- If `v.index() != w.index()`, returns `false`;
- otherwise if `v.valueless_by_exception()`, returns `true`;
- otherwise returns `*std::get_if<v.index()>(std::addressof(v)) ==
  *std::get_if<v.index()>(std::addressof(w))`. The behavior is undefined(until
  C++20)The program is ill-formed(since C++20) if
  `*std::get_if<i>(std::addressof(v)) == *std::get_if<i>(std::addressof(w))` is
  not a valid expression returning a type convertible to bool, for any `i`.

2) Inequality operator for variants:

- If `v.index() != w.index()`, returns `true`;
- otherwise if `v.valueless_by_exception()`, returns `false`;
- otherwise returns `*std::get_if<v.index()>(std::addressof(v)) !=
  *std::get_if<v.index()>(std::addressof(w))`. The behavior is undefined(until
  C++20)The program is ill-formed(since C++20) if
  `*std::get_if<i>(std::addressof(v)) != *std::get_if<i>(std::addressof(w))` is
  not a valid expression returning a type convertible to bool, for any `i`.

3) Less-than operator for variants:

- If `w.valueless_by_exception()`, returns `false`;
- otherwise if `v.valueless_by_exception()`, returns `true`;
- otherwise if `v.index() < w.index()`, returns `true`;
- otherwise if `v.index() > w.index()`, returns `false`;
- otherwise returns `*std::get_if<v.index()>(std::addressof(v)) <
  *std::get_if<v.index()>(std::addressof(w))`. The behavior is undefined(until
  C++20)The program is ill-formed(since C++20) if
  `*std::get_if<i>(std::addressof(v)) < *std::get_if<i>(std::addressof(w))` is
  not a valid expression returning a type convertible to bool, for any `i`.

4) Greater-than operator for variants:

- If `v.valueless_by_exception()`, returns `false`;
- otherwise if `w.valueless_by_exception()`, returns `true`;
- otherwise if `v.index() > w.index()`, returns `true`;
- otherwise if `v.index() < w.index()`, returns `false`;
- otherwise returns `*std::get_if<v.index()>(std::addressof(v)) >
  *std::get_if<v.index()>(std::addressof(w))`. The behavior is undefined(until
  C++20)The program is ill-formed(since C++20) if
  `*std::get_if<i>(std::addressof(v)) > *std::get_if<i>(std::addressof(w))` is
  not a valid expression returning a type convertible to bool, for any `i`.

5) Less-equal operator for variants:

- If `v.valueless_by_exception()`, returns `true`;
- otherwise if `w.valueless_by_exception()`, returns `false`;
- otherwise if `v.index() < w.index()`, returns `true`;
- otherwise if `v.index() > w.index()`, returns `false`;
- otherwise returns `*std::get_if<v.index()>(std::addressof(v)) <=
  *std::get_if<v.index()>(std::addressof(w))`. The behavior is undefined(until
  C++20)The program is ill-formed(since C++20) if
  `*std::get_if<i>(std::addressof(v)) <= *std::get_if<i>(std::addressof(w))` is
  not a valid expression returning a type convertible to bool, for any `i`.

6) Greater-equal operator for variants:

- If `w.valueless_by_exception()`, returns `true`;
- otherwise if `v.valueless_by_exception()`, returns `false`;
- otherwise if `v.index() > w.index()`, returns `true`;
- otherwise if `v.index() < w.index()`, returns `false`;
- otherwise `*std::get_if<v.index()>(std::addressof(v)) >=
  *std::get_if<v.index()>(std::addressof(w))`.The behavior is undefined(until
  C++20)The program is ill-formed(since C++20) if
  `*std::get_if<i>(std::addressof(v)) >= *std::get_if<i>(std::addressof(w))` is
  not a valid expression returning a type convertible to bool, for any `i`.

7) Three-way comparison operator for variants:

- If both `v.valueless_by_exception()` and `w.valueless_by_exception()` are
  `true`, returns `std::strong_ordering::equal`;
- otherwise if `v.valueless_by_exception()` is `true`, returns
  `std::strong_ordering::less`;
- otherwise if `w.valueless_by_exception()` is `true`, returns
  `std::strong_ordering::greater`;
- otherwise if `v.index() != w.index()`, returns `v.index() <=> w.index()`;
- otherwise equivalent to `*std::get_if<v.index()>(std::addressof(v)) <=>
  *std::get_if<v.index()>(std::addressof(w))`.

### Parameters

- **v,w** — variants to compare

### Return value

The result of the comparison as described above.

### Example

```cpp
#include <iostream>
#include <string>
#include <variant>

int main()
{
    std::cout << std::boolalpha;
    std::string cmp;
    bool result;

    auto print2 = [&cmp, &result](const auto& lhs, const auto& rhs) {
        std::cout << lhs << ' ' << cmp << ' ' << rhs << " : " << result << '\n';
    };

    std::variant<int, std::string> v1, v2;

    std::cout << "operator==\n";
    {
        cmp = "==";

        // by default v1 = 0, v2 = 0;
        result = v1 == v2; // true
        std::visit(print2, v1, v2);

        v1 = v2 = 1;
        result = v1 == v2; // true
        std::visit(print2, v1, v2);

        v2 = 2;
        result = v1 == v2; // false
        std::visit(print2, v1, v2);

        v1 = "A";
        result = v1 == v2; // false: v1.index == 1, v2.index == 0
        std::visit(print2, v1, v2);

        v2 = "B";
        result = v1 == v2; // false
        std::visit(print2, v1, v2);

        v2 = "A";
        result = v1 == v2; // true
        std::visit(print2, v1, v2);
    }

    std::cout << "operator<\n";
    {
        cmp = "<";

        v1 = v2 = 1;
        result = v1 < v2; // false
        std::visit(print2, v1, v2);

        v2 = 2;
        result = v1 < v2; // true
        std::visit(print2, v1, v2);

        v1 = 3;
        result = v1 < v2; // false
        std::visit(print2, v1, v2);

        v1 = "A"; v2 = 1;
        result = v1 < v2; // false: v1.index == 1, v2.index == 0
        std::visit(print2, v1, v2);

        v1 = 1; v2 = "A";
        result = v1 < v2; // true: v1.index == 0, v2.index == 1
        std::visit(print2, v1, v2);

        v1 = v2 = "A";
        result = v1 < v2; // false
        std::visit(print2, v1, v2);

        v2 = "B";
        result = v1 < v2; // true
        std::visit(print2, v1, v2);

        v1 = "C";
        result = v1 < v2; // false
        std::visit(print2, v1, v2);
    }

    {
        std::variant<int, std::string> v1;
        std::variant<std::string, int> v2;
    //  v1 == v2;  // Compilation error: no known conversion
    }

    // TODO: C++20 three-way comparison operator <=> for variants
}
```

Output:

```text
operator==
0 == 0 : true
1 == 1 : true
1 == 2 : false
A == 2 : false
A == B : false
A == A : true
operator<
1 < 1 : false
1 < 2 : true
3 < 2 : false
A < 1 : false
1 < A : true
A < A : false
A < B : true
C < B : false
```

### See also

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++17)(C++17)(C++17)(C++17)(C++17)(C++17)(C++20)** — compares `optional`
  objects (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/variant/operator_cmp*
