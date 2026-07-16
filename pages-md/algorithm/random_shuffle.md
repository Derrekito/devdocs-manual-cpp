# std::random_shuffle, std::shuffle

```cpp
template< class RandomIt >
void random_shuffle( RandomIt first, RandomIt last );  // (1) (deprecated in C++14) (removed in C++17)
template< class RandomIt, class RandomFunc >
void random_shuffle( RandomIt first, RandomIt last, RandomFunc& r );  // (until C++11)
template< class RandomIt, class RandomFunc >
void random_shuffle( RandomIt first, RandomIt last, RandomFunc&& r );  // (since C++11) (deprecated in C++14) (removed in C++17)
template< class RandomIt, class URBG >
void shuffle( RandomIt first, RandomIt last, URBG&& g );  // (3) (since C++11)
```

Reorders the elements in the given range `[``first``,``last``)` such that each
possible permutation of those elements has equal probability of appearance.

1) The source of randomness is implementation-defined, but the function
   `std::rand` is often used.

2) The source of randomness is the function object `r`.

3) The source of randomness is the object `g`.

### Parameters

- **first, last** — the range of elements to shuffle randomly
- **r** — function object returning a randomly chosen value of type convertible
  to std::iterator_traits<RandomIt>::difference_type in the interval
  `[``​0​``,``n``)` if invoked as `r(n)`
- **g** — a UniformRandomBitGenerator whose result type is convertible to
  std::iterator_traits<RandomIt>::difference_type

**Type requirements**

**-`RandomIt` must meet the requirements of ValueSwappable and LegacyRandomAccessIterator.**

**-`std::remove_reference_t<URBG>` must meet the requirements of UniformRandomBitGenerator.**

### Return value

(none)

### Complexity

Linear in the distance between `first` and `last`.

### Notes

Note that the implementation is not dictated by the standard, so even if you use
exactly the same `RandomFunc` or `URBG` (Uniform Random Number Generator) you
may get different results with different standard library implementations.

The reason for removing `std::random_shuffle` in C++17 is that the iterator-only
version usually depends on `std::rand`, which is now also discussed for
deprecation. (`std::rand` should be replaced with the classes of the `<random>`
header, as `std::rand` is *considered harmful*.) In addition, the iterator-only
`std::random_shuffle` version usually depends on a global state. The
`std::shuffle`'s shuffle algorithm is the preferred replacement, as it uses a
`URBG` as its 3rd parameter.

### Possible implementation

See also the implementations in libstdc++ and libc++.

```cpp
template<class RandomIt>
void random_shuffle(RandomIt first, RandomIt last)
{
    typedef typename std::iterator_traits<RandomIt>::difference_type diff_t;

    for (diff_t i = last - first - 1; i > 0; --i)
    {
        using std::swap;
        swap(first[i], first[std::rand() % (i + 1)]);
        // rand() % (i + 1) is not actually correct, because the generated number is
        // not uniformly distributed for most values of i. The correct code would be
        // a variation of the C++11 std::uniform_int_distribution implementation.
    }
}
```

```cpp
template<class RandomIt, class RandomFunc>
void random_shuffle(RandomIt first, RandomIt last, RandomFunc&& r)
{
    typedef typename std::iterator_traits<RandomIt>::difference_type diff_t;

    for (diff_t i = last - first - 1; i > 0; --i)
    {
        using std::swap;
        swap(first[i], first[r(i + 1)]);
    }
}
```

```cpp
template<class RandomIt, class URBG>
void shuffle(RandomIt first, RandomIt last, URBG&& g)
{
    typedef typename std::iterator_traits<RandomIt>::difference_type diff_t;
    typedef std::uniform_int_distribution<diff_t> distr_t;
    typedef typename distr_t::param_type param_t;

    distr_t D;
    for (diff_t i = last - first - 1; i > 0; --i)
    {
        using std::swap;
        swap(first[i], first[D(g, param_t(0, i))]);
    }
}
```

### Example

Randomly shuffles the sequence `[`1`,`10`]` of integers:

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <random>
#include <vector>

int main()
{
    std::vector<int> v{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

    std::random_device rd;
    std::mt19937 g(rd());

    std::shuffle(v.begin(), v.end(), g);

    std::copy(v.begin(), v.end(), std::ostream_iterator<int>(std::cout, " "));
    std::cout << '\n';
}
```

Possible output:

```text
8 6 10 4 2 3 7 1 9 5
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 395 | C++98 | the source of randomness of overload (1) was not specified,
      and `std::rand` could not be the source due to the C library requirement |
      it is implementation-defined, and using `std::rand` is allowed
  LWG 552 (N2423) | C++98 | `r` was not required to be the source of randomness
      of overload (2)[1] | required

1. Overload (3) has the same defect, but that part of the resolution is not
   applicable to C++98.

### See also

- **next_permutation** — generates the next greater lexicographic permutation of
  a range of elements (function template)
- **prev_permutation** — generates the next smaller lexicographic permutation of
  a range of elements (function template)
- **ranges::shuffle (C++20)** — randomly re-orders elements in a range
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/random_shuffle*
