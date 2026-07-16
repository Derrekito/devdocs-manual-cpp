# std::bitset<N>::reference

```cpp
class reference;
```

The `std::bitset` class includes `std::bitset::reference` as a
publicly-accessible nested class. This class is used as a proxy object to allow
users to interact with individual bits of a bitset, since standard C++ types
(like references and pointers) are not built with enough precision to specify
individual bits.

The primary use of `std::bitset::reference` is to provide an lvalue that can be
returned from `operator[]`.

Any reads or writes to a bitset that happen via a `std::bitset::reference`
potentially read or write to the entire underlying bitset.

### Member functions

- **(constructor)** — constructs the reference. Accessible only to `std::bitset`
  itself (private member function)
- ****(destructor)**** — destroys the reference (public member function)
- **operator=** — assigns a `bool` to the referenced bit (public member
  function)
- ****operator bool**** — returns the referenced bit (public member function)
- **operator~** — returns inverted referenced bit (public member function)
- **flip** — flips the referenced bit (public member function)

## std::bitset<N>::reference::~reference

```cpp
~reference();  // (constexpr since C++23)
```

Destroys the reference.

## std::bitset<N>::reference::operator=

```cpp
reference& operator=( bool x );  // (until C++11)
reference& operator=( bool x ) noexcept;  // (since C++11) (constexpr since C++23)
reference& operator=( const reference& x );  // (until C++11)
reference& operator=( const reference& x ) noexcept;  // (since C++11) (constexpr since C++23)
```

Assigns a value to the referenced bit.

### Parameters

- **x** — value to assign

### Return value

`*this`

## std::bitset<N>::reference::operator bool

```cpp
operator bool() const;  // (until C++11)
operator bool() const noexcept;  // (since C++11) (constexpr since C++23)
```

Returns the value of the referenced bit.

### Parameters

(none)

### Return value

The referenced bit.

## std::bitset<N>::reference::operator~

```cpp
bool operator~() const;  // (until C++11)
bool operator~() const noexcept;  // (since C++11) (constexpr since C++23)
```

Returns the inverse of the referenced bit.

### Parameters

(none)

### Return value

The inverse of the referenced bit.

## std::bitset<N>::reference::flip

```cpp
reference& flip();  // (until C++11)
reference& flip() noexcept;  // (since C++11) (constexpr since C++23)
```

Inverts the referenced bit.

### Parameters

(none)

### Return value

`*this`

### Example

```cpp
#include <bitset>
#include <iostream>

int main()
{
    std::bitset<4> bs{0b1110};

    std::bitset<4>::reference ref = bs[2];

    auto info = [&](int id)
    {
        std::cout << id << ") bs: " << bs << "; ref bit: " << ref << '\n';
    };

    info(1);
    ref = false;
    info(2);
    ref = true;
    info(3);
    ref.flip();
    info(4);
    ref = bs[1]; // operator=( const reference& x )
    info(5);

    std::cout << "6) ~ref bit: " << ~ref << '\n';
}
```

Output:

```text
1) bs: 1110; ref bit: 1
2) bs: 1010; ref bit: 0
3) bs: 1110; ref bit: 1
4) bs: 1010; ref bit: 0
5) bs: 1110; ref bit: 1
6) ~ref bit: 0
```

### See also

- **operator[]** — accesses specific bit (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/bitset/reference*
