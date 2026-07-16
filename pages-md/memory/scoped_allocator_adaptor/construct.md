# std::scoped_allocator_adaptor<OuterAlloc,InnerAlloc...>::construct

```cpp
template< class T, class... Args >
void construct( T* p, Args&&... args );  // (1)
template< class T1, class T2, class... Args1, class... Args2 >
void construct( std::pair<T1, T2>* p,
                std::piecewise_construct_t,
                std::tuple<Args1...> x,
                std::tuple<Args2...> y );  // (2) (until C++20)
template< class T1, class T2 >
void construct( std::pair<T1, T2>* p );  // (3) (until C++20)
template< class T1, class T2, class U, class V >
void construct( std::pair<T1, T2>* p, U&& x, V&& y );  // (4) (until C++20)
template< class T1, class T2, class U, class V >
void construct( std::pair<T1, T2>* p, const std::pair<U, V>& xy );  // (5) (until C++20)
template< class T1, class T2, class U, class V >
void construct( std::pair<T1, T2>* p, std::pair<U, V>&& xy );  // (6) (until C++20)
template< class T1, class T2, class NonPair >
void construct( std::pair<T1, T2>* p, NonPair&& non_pair );  // (7) (until C++20)
```

Constructs an object in allocated, but not initialized storage pointed to by `p`
using OuterAllocator and the provided constructor arguments. If the object is of
type that itself uses allocators, or if it is std::pair, passes InnerAllocator
down to the constructed object.

First, retrieve the outermost allocator `OUTERMOST` by calling
`this->outer_allocator()`, and then calling the `outer_allocator()` member
function recursively on the result of this call until reaching an allocator that
has no such member function.

Define `OUTERMOST_ALLOC_TRAITS`(x) as
`std::allocator_traits<std::remove_reference_t<decltype(OUTERMOST(x))>>`

1) Creates an object of the given type `T` by means of uses-allocator
   construction at the uninitialized memory location indicated by p, using
   `OUTERMOST` as the allocator. After adjustment for uses-allocator convention
   expected by T's constructor, calls
   `OUTERMOST_ALLOC_TRAITS(*this)::construct`.

This overload participates in overload resolution only if `U` is not a
specialization of `std::pair`.
*(until C++20)*

Equivalent to
`std::apply( [p, this](auto&&... newargs) {
OUTERMOST_ALLOC_TRAITS(*this)::construct( OUTERMOST(*this), p,
std::forward<decltype(newargs)>(newargs)...); },
std::uses_allocator_construction_args( inner_allocator(),
std::forward<Args>(args)... ) );`
*(since C++20)*

2) First, if either `T1` or `T2` is allocator-aware, modifies the tuples `x` and
`y` to include the appropriate inner allocator, resulting in the two new tuples
`xprime` and `yprime`, according to the following three rules:
2a) if `T1` is not allocator-aware
(`std::uses_allocator<T1, inner_allocator_type>::value == false`), then `xprime`
is `std::tuple<Args1&&...>(std::move(x))`. (It is also required that
`std::is_constructible<T1, Args1...>::value == true`).
2b) if `T1` is allocator-aware (`std::uses_allocator<T1,
inner_allocator_type>::value == true`), and its constructor takes an allocator
tag
`std::is_constructible<T1, std::allocator_arg_t, inner_allocator_type&,
Args1...>::value == true`,
then `xprime` is
`std::tuple_cat(std::tuple<std::allocator_arg_t, inner_allocator_type&>(
std::allocator_arg, inner_allocator() ), std::tuple<Args1&&...>(std::move(x)))`
2c) if `T1` is allocator-aware (`std::uses_allocator<T1,
inner_allocator_type>::value == true`), and its constructor takes the allocator
as the last argument
`std::is_constructible<T1, Args1..., inner_allocator_type&>::value == true`,
then `xprime` is
`std::tuple_cat(std::tuple<Args1&&...>(std::move(x)),
std::tuple<inner_allocator_type&>(inner_allocator()))`.
Same rules apply to `T2` and the replacement of `y` with `yprime`. Once `xprime`
and `yprime` are constructed, constructs the pair `p` in allocated storage by
calling
`std::allocator_traits<O>::construct(OUTERMOST, p, std::piecewise_construct,
std::move(xprime), std::move(yprime));`
3) Equivalent to
`construct(p, std::piecewise_construct, std::tuple<>(), std::tuple<>())`, that
is, passes the inner allocator on to the pair's member types if they accept
them.
4) Equivalent to
`construct(p, std::piecewise_construct,
std::forward_as_tuple(std::forward<U>(x)),
std::forward_as_tuple(std::forward<V>(y)))`
5) Equivalent to
`construct(p, std::piecewise_construct, std::forward_as_tuple(xy.first),
std::forward_as_tuple(xy.second))`
6) Equivalent to
`construct(p, std::piecewise_construct,
std::forward_as_tuple(std::forward<U>(xy.first)),
std::forward_as_tuple(std::forward<V>(xy.second)))`
7) This overload participates in overload resolution only if given the
exposition-only function template
`template<class A, class B>void /*deduce-as-pair*/(const std::pair<A, B>&);`,
`/*deduce-as-pair*/(non_pair)` is ill-formed when considered as an unevaluated
operand.
Equivalent to `construct<T1, T2, T1, T2>(p, std::forward<NonPair>(non_pair));`.
*(until C++20)*

### Parameters

- **p** — pointer to allocated, but not initialized storage
- **args...** — the constructor arguments to pass to the constructor of `T`
- **x** — the constructor arguments to pass to the constructor of `T1`
- **y** — the constructor arguments to pass to the constructor of `T2`
- **xy** — the pair whose two members are the constructor arguments for `T1` and
  `T2`
- **non_pair** — non-`pair` argument to convert to `pair` for further
  construction

### Return value

(none)

### Notes

This function is called (through `std::allocator_traits`) by any allocator-aware
object, such as `std::vector`, that was given a `std::scoped_allocator_adaptor`
as the allocator to use. Since `inner_allocator` is itself an instance of
`std::scoped_allocator_adaptor`, this function will also be called when the
allocator-aware objects constructed through this function start constructing
their own members.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2975 | C++11 | first overload is mistakenly used for pair construction in
      some cases | constrained to not accept pairs
  P0475R1 | C++11 | pair piecewise construction may copy the arguments |
      transformed to tuples of references to avoid copy
  LWG 3525 | C++11 | no overload could handle non-`pair` types convertible to
      `pair` | reconstructing overload added

### See also

- **construct [static]** — constructs an object in the allocated storage
  (function template)
- **construct (until C++20)** — constructs an object in allocated storage
  (public member function of `std::allocator<T>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/scoped_allocator_adaptor/construct*
