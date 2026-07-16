# std::valarray<T>::operator+,-,~,!

```cpp
valarray<T> operator+() const;  // (1)
valarray<T> operator-() const;  // (2)
valarray<T> operator~() const;  // (3)
valarray<bool> operator!() const;  // (4)
```

Applies unary operators to each element in the numeric array.

### Parameters

(none)

### Return value

A numeric array containing elements with values obtained by applying
corresponding operator to the values in `*this`.

### Exceptions

May throw implementation-defined exceptions.

### Notes

Each of the operators can only be instantiated if the following requirements are
met:

- The indicated operator can be applied to type `T`.
- The result value can be unambiguously converted to `T` (1-3) or `bool` (4).

The function can be implemented with the return type different from
`std::valarray`. In this case, the replacement type has the following
properties:

- All const member functions of `std::valarray` are provided.
- `std::valarray`, `std::slice_array`, `std::gslice_array`, `std::mask_array`
  and `std::indirect_array` can be constructed from the replacement type.
- For every function taking a const std::valarray<T>& except `begin()` and
  `end()`(since C++11), identical functions taking the replacement types shall
  be added;
- For every function taking two const std::valarray<T>& arguments, identical
  functions taking every combination of const std::valarray<T>& and replacement
  types shall be added.
- The return type does not add more than two levels of template nesting over the
  most deeply-nested argument type.

### Example

```cpp
#include <iostream>
#include <string_view>
#include <valarray>

template<typename T>
void print(std::string_view const note,
           std::valarray<T> const vala, // by-value, see Notes above
           std::string_view const term = "\n")
{
    std::cout << note << std::boolalpha << std::showpos;
    for (T const element : vala)
        std::cout << '\t' << element;
    std::cout << term;
}

int main()
{
    std::valarray<int> x{1, 2, 3, 4};
    print<int>("x: ", x);
    print<int>("+x: ", +x);
    print<int>("+ + x: ", + + x);
    print<int>("-x: ", -x);
    print<int>("- - x: ", - - x, "\n\n");

    std::valarray<short> y{0, 1, -1, 0x7fff};
    print<short>("y: ", y);
    print<short>("~y: ", ~y);
    print<short>("~~y: ", ~~y, "\n\n");

    std::valarray<bool> z{true, false};
    print<bool>("z: ", z);
    print<bool>("!z: ", !z);
    print<bool>("!!z: ", !!z);
}
```

Possible output:

```text
x:      +1      +2      +3      +4
+x:     +1      +2      +3      +4
+ + x:  +1      +2      +3      +4
-x:     -1      -2      -3      -4
- - x:  +1      +2      +3      +4

y:      +0      +1      -1      +32767
~y:     -1      -2      +0      -32768
~~y:    +0      +1      -1      +32767

z:      true    false
!z:     false   true
!!z:    true    false
```

### See also

-
  **operator+=operator-=operator*=operator/=operator%=operator&=operator|=operator^=operator<<=operator>>=**
  — applies compound assignment operator to each element of the valarray (public
  member function)
-
  **operator+operator-operator*operator/operator%operator&operator|operator^operator<<operator>>operator&&operator||**
  — applies binary operators to each element of two valarrays, or a valarray and
  a value (function template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/operator_arith*
