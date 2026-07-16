# std::ranges::zip_view<Views...>::*iterator*<Const>::operator++,--,+=,-=

```cpp
constexpr /*iterator*/& operator++();  // (1) (since C++23)
constexpr void operator++( int );  // (2) (since C++23)
constexpr /*iterator*/ operator++( int )
  requires /*all-forward*/<Const, Views...>;  // (3) (since C++23)
constexpr /*iterator*/& operator--()
  requires /*all-bidirectional*/<Const, Views...>;  // (4) (since C++23)
constexpr /*iterator*/ operator--( int )
  requires /*all-bidirectional*/<Const, Views...>;  // (5) (since C++23)
constexpr /*iterator*/& operator+=( difference_type n )
  requires /*all-random-access*/<Const, Views...>;  // (6) (since C++23)
constexpr /*iterator*/& operator-=( difference_type n )
  requires /*all-random-access*/<Const, Views...>;  // (7) (since C++23)
```

Increments or decrements each of the underlying `is_...` iterators in the
underlying tuple-like object `current_`.

1) Equivalent to `/*tuple-for-each*/([](auto& i) { ++i; }, current_); return
   *this;`

2) Equivalent to `++*this;`

3) Equivalent to `auto tmp = *this; ++*this; return tmp;`

4) Equivalent to `/*tuple-for-each*/([](auto& i) { --i; }, current_); return
   *this;`

5) Equivalent to `auto tmp = *this; --*this; return tmp;`

6) Equivalent to `/*tuple-for-each*/([&]<class I>(I& i) { i +=
   iter_difference_t<I>(x); }, current_); return *this;`

7) Equivalent to `/*tuple-for-each*/([&]<class I>(I& i) { i -=
   iter_difference_t<I>(x); }, current_); return *this;`

### Parameters

- **n** — position relative to current location

### Return value

1,4,6,7) `*this`

2) (none)

3,5) a copy of `*this` that was made before the change

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_view/iterator/operator_arith*
