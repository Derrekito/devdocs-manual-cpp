# std::basic_regex<CharT,Traits>::operator=

```cpp
basic_regex& operator=( const basic_regex& other );  // (1) (since C++11)
basic_regex& operator=( basic_regex&& other ) noexcept;  // (2) (since C++11)
basic_regex& operator=( const CharT* ptr );  // (3) (since C++11)
basic_regex& operator=( std::initializer_list<CharT> il );  // (4) (since C++11)
template< class ST, class SA >
basic_regex& operator=( const std::basic_string<CharT,ST,SA>& p );  // (5) (since C++11)
```

Assigns the contents.

1) Copy assignment operator. Assigns the contents of `other`. Equivalent to
   `assign(other);`.

2) Move assignment operator. Assigns the contents of `other` using move
   semantics. `other` is in valid, but unspecified state after the operation.
   Equivalent to `assign(other);`.

3) Assigns a null-terminated character string pointed to by `ptr`. Equivalent to
   `assign(ptr);`.

4) Assigns characters contained in initializer list `il`. Equivalent to
   `assign(il);`.

5) Assigns the contents of the string `p`. Equivalent to `assign(p);`.

### Parameters

- **other** — another regex object
- **ptr** — pointer to a null-terminated character string
- **il** — initializer list containing characters to assign
- **p** — string containing characters to assign

### Return value

`*this`

### Exceptions

1) May throw implementation-defined exceptions.

3-5) `std::regex_error` if the supplied regular expression is not valid. The
   object is not modified in that case.

### See also

- **assign** — assigns the contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/regex/basic_regex/operator%3D*
