# std::ranges::cartesian_product_view<First, Vs...>::*iterator*<Const>::*next*, std::ranges::cartesian_product_view<First, Vs...>::*iterator*<Const>::*prev*, std::ranges::cartesian_product_view<First, Vs...>::*iterator*<Const>::*distance_from*

## std::ranges::cartesian_product_view::*iterator*::*next*

```cpp
template< std::size_t N = sizeof...(Vs) >
constexpr void /*next*/();  // (since C++23) (exposition only*)
```

If called with default template parameter, recursively generates the next
element (the tuple of iterators) in `cartesian_product_view`.

Let `current_` denote the underlying tuple of iterators. Equivalent to:

```cpp
auto& it = std::get<N>(current_);
++it;
if constexpr (N > 0)
{
    if (it == ranges::end(std::get<N>(parent_->bases_)))
    {
        it = ranges::begin(std::get<N>(parent_->bases_));
        next<N - 1>();
    }
}
```

Used in the following non-static member functions:

- ranges::cartesian_product_view::`operator+`

## std::ranges::cartesian_product_view::*iterator*::*prev*

```cpp
template< std::size_t N = sizeof...(Vs) >
constexpr void /*prev*/();  // (since C++23) (exposition only*)
```

If called with default template parameter, recursively generates the previous
element (the tuple of iterators) in `cartesian_product_view`.

Let `current_` denote the underlying tuple of iterators. Equivalent to:

```cpp
auto& it = std::get<N>(current_);
if constexpr (N > 0)
{
    if (it == ranges::begin(std::get<N>(parent_->bases_)))
    {
        it = /*cartesian-common-arg-end*/(std::get<N>(parent_->bases_));
        prev<N - 1>();
    }
}
--it;
```

Used in the following non-static member functions:

- ranges::cartesian_product_view::`operator-`

## std::ranges::cartesian_product_view::*iterator*::*distance_from*

```cpp
template< class Tuple >
constexpr difference_type
    /*distance-from*/( const Tuple& t ) const;  // (since C++23) (exposition only*)
```

Returns the "distance" (i.e., number of "hops") between two iterators.

Let:

- `parent_` be the underlying pointer to `cartesian_product_view`
- `/*scaled-size*/(N)` be: the product of
  `static_cast<difference_type>(ranges::size(std::get<N>(parent_->bases_)))` and
  `/*scaled-size*/(N + 1)` if `N ≤ sizeof...(Vs)`, otherwise
  `static_cast<difference_type>(1);`
- `/*scaled-distance*/(N)` be the product of
  `static_cast<difference_type>(std::get<N>(current_) - std::get<N>(t))` and
  `/*scaled-size*/(N + 1);`
- `/*scaled-sum*/` be the sum of `/*scaled-distance*/(N)` for every integer `0 ≤
  N ≤ sizeof...(Vs)`.

Returns: `/*scaled-sum*/`.

The behavior is undefined if `/*scaled-sum*/` cannot be represented by
`difference_type`.

Used in the following functions:

- `operator-`(const /*iterator*/&, const /*iterator*/&)
- `operator-`(const /*iterator*/&, std::default_sentinel_t)

### Parameters

- **t** — a tuple of iterators to find the distance to

---
*Source: https://en.cppreference.com/w/cpp/ranges/cartesian_product_view/iterator/helpers*
