# Standard library header <set>

This header is part of the containers library.

**Includes**

- **<compare> (C++20)** — Three-way comparison operator support
- **<initializer_list> (C++11)** — `std::initializer_list` class template

**Classes**

- **set** — collection of unique keys, sorted by keys (class template)
- **multiset** — collection of keys, sorted by keys (class template)

**Functions**

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values of
  two `sets` (function template)
- **std::swap(std::set)** — specializes the `std::swap` algorithm (function
  template)
- **erase_if(std::set) (C++20)** — erases all elements satisfying specific
  criteria (function template)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values of
  two `multisets` (function template)
- **std::swap(std::multiset)** — specializes the `std::swap` algorithm (function
  template)
- **erase_if(std::multiset) (C++20)** — erases all elements satisfying specific
  criteria (function template)

**Range access**

- **begincbegin (C++11)(C++14)** — returns an iterator to the beginning of a
  container or array (function template)
- **endcend (C++11)(C++14)** — returns an iterator to the end of a container or
  array (function template)
- **rbegincrbegin (C++14)** — returns a reverse iterator to the beginning of a
  container or array (function template)
- **rendcrend (C++14)** — returns a reverse end iterator for a container or
  array (function template)
- **sizessize (C++17)(C++20)** — returns the size of a container or array
  (function template)
- **empty (C++17)** — checks whether the container is empty (function template)
- **data (C++17)** — obtains the pointer to the underlying array (function
  template)

### Synopsis

```cpp
#include <compare>
#include <initializer_list>

namespace std {
  // class template set
  template<class Key, class Compare = less<Key>, class Allocator = allocator<Key>>
    class set;

  template<class Key, class Compare, class Allocator>
    bool operator==(const set<Key, Compare, Allocator>& x,
                    const set<Key, Compare, Allocator>& y);
  template<class Key, class Compare, class Allocator>
    __synth_three_way_result<Key> operator<=>(const set<Key, Compare, Allocator>& x,
                                            const set<Key, Compare, Allocator>& y);

  template<class Key, class Compare, class Allocator>
    void swap(set<Key, Compare, Allocator>& x,
              set<Key, Compare, Allocator>& y)
      noexcept(noexcept(x.swap(y)));

  // erasure for set
  template<class Key, class Compare, class Allocator, class Predicate>
    typename set<Key, Compare, Allocator>::size_type
      erase_if(set<Key, Compare, Allocator>& c, Predicate pred);

  // class template multiset
  template<class Key, class Compare = less<Key>, class Allocator = allocator<Key>>
    class multiset;

  template<class Key, class Compare, class Allocator>
    bool operator==(const multiset<Key, Compare, Allocator>& x,
                    const multiset<Key, Compare, Allocator>& y);
  template<class Key, class Compare, class Allocator>
    __synth_three_way_result<Key> operator<=>(const multiset<Key, Compare, Allocator>& x,
                                            const multiset<Key, Compare, Allocator>& y);

  template<class Key, class Compare, class Allocator>
    void swap(multiset<Key, Compare, Allocator>& x,
              multiset<Key, Compare, Allocator>& y)
      noexcept(noexcept(x.swap(y)));

  // erasure for multiset
  template<class Key, class Compare, class Allocator, class Predicate>
    typename multiset<Key, Compare, Allocator>::size_type
      erase_if(multiset<Key, Compare, Allocator>& c, Predicate pred);

  namespace pmr {
    template<class Key, class Compare = less<Key>>
      using set = std::set<Key, Compare, polymorphic_allocator<Key>>;

    template<class Key, class Compare = less<Key>>
      using multiset = std::multiset<Key, Compare, polymorphic_allocator<Key>>;
  }
}
```

#### Class template `std::set`

```cpp
namespace std {
  template<class Key, class Compare = less<Key>,
           class Allocator = allocator<Key>>
  class set {
  public:
    // types
    using key_type               = Key;
    using key_compare            = Compare;
    using value_type             = Key;
    using value_compare          = Compare;
    using allocator_type         = Allocator;
    using pointer                = typename allocator_traits<Allocator>::pointer;
    using const_pointer          = typename allocator_traits<Allocator>::const_pointer;
    using reference              = value_type&;
    using const_reference        = const value_type&;
    using size_type              = /* implementation-defined */;
    using difference_type        = /* implementation-defined */;
    using iterator               = /* implementation-defined */;
    using const_iterator         = /* implementation-defined */;
    using reverse_iterator       = std::reverse_iterator<iterator>;
    using const_reverse_iterator = std::reverse_iterator<const_iterator>;
    using node_type              = /* unspecified */;
    using insert_return_type     = __insert_return_type<iterator, node_type>;

    // construct/copy/destroy
    set() : set(Compare()) { }
    explicit set(const Compare& comp, const Allocator& = Allocator());
    template<class InputIt>
      set(InputIt first, InputIt last,
          const Compare& comp = Compare(), const Allocator& = Allocator());
    template<container-compatible-range<value_type> R>
      set(from_range_t, R&& rg, const Compare& comp = Compare(),
          const Allocator& = Allocator());
    set(const set& x);
    set(set&& x);
    explicit set(const Allocator&);
    set(const set&, const type_identity_t<Allocator>&);
    set(set&&, const type_identity_t<Allocator>&);
    set(initializer_list<value_type>, const Compare& = Compare(),
        const Allocator& = Allocator());
    template<class InputIt>
      set(InputIt first, InputIt last, const Allocator& a)
        : set(first, last, Compare(), a) { }
    template<container-compatible-range<value_type> R>
      set(from_range_t, R&& rg, const Allocator& a))
        : set(from_range, std::forward<R>(rg), Compare(), a) { }
    set(initializer_list<value_type> il, const Allocator& a)
      : set(il, Compare(), a) { }
    ~set();
    set& operator=(const set& x);
    set& operator=(set&& x)
      noexcept(allocator_traits<Allocator>::is_always_equal::value &&
               is_nothrow_move_assignable_v<Compare>);
    set& operator=(initializer_list<value_type>);
    allocator_type get_allocator() const noexcept;

    // iterators
    iterator               begin() noexcept;
    const_iterator         begin() const noexcept;
    iterator               end() noexcept;
    const_iterator         end() const noexcept;

    reverse_iterator       rbegin() noexcept;
    const_reverse_iterator rbegin() const noexcept;
    reverse_iterator       rend() noexcept;
    const_reverse_iterator rend() const noexcept;

    const_iterator         cbegin() const noexcept;
    const_iterator         cend() const noexcept;
    const_reverse_iterator crbegin() const noexcept;
    const_reverse_iterator crend() const noexcept;

    // capacity
    [[nodiscard]] bool empty() const noexcept;
    size_type size() const noexcept;
    size_type max_size() const noexcept;

    // modifiers
    template<class... Args> pair<iterator, bool> emplace(Args&&... args);
    template<class... Args> iterator emplace_hint(const_iterator position,
                                                  Args&&... args);
    pair<iterator,bool> insert(const value_type& x);
    pair<iterator,bool> insert(value_type&& x);
    iterator insert(const_iterator position, const value_type& x);
    iterator insert(const_iterator position, value_type&& x);
    template<class InputIt>
      void insert(InputIt first, InputIt last);
    template<container-compatible-range<value_type> R>
      void insert_range(R&& rg);
    void insert(initializer_list<value_type>);

    node_type extract(const_iterator position);
    node_type extract(const key_type& x);
    template<class K> node_type extract(K&& x);
    insert_return_type insert(node_type&& nh);
    iterator           insert(const_iterator hint, node_type&& nh);

    iterator  erase(iterator position)
      requires (!same_as<iterator, const_iterator>);
    iterator  erase(const_iterator position);
    size_type erase(const key_type& x);
    template<class K> size_type erase(K&& x);
    iterator  erase(const_iterator first, const_iterator last);
    void      swap(set&)
      noexcept(allocator_traits<Allocator>::is_always_equal::value &&
               is_nothrow_swappable_v<Compare>);
    void      clear() noexcept;

    template<class C2>
      void merge(set<Key, C2, Allocator>& source);
    template<class C2>
      void merge(set<Key, C2, Allocator>&& source);
    template<class C2>
      void merge(multiset<Key, C2, Allocator>& source);
    template<class C2>
      void merge(multiset<Key, C2, Allocator>&& source);

    // observers
    key_compare key_comp() const;
    value_compare value_comp() const;

    // set operations
    iterator       find(const key_type& x);
    const_iterator find(const key_type& x) const;
    template<class K> iterator       find(const K& x);
    template<class K> const_iterator find(const K& x) const;

    size_type      count(const key_type& x) const;
    template<class K> size_type count(const K& x) const;

    bool           contains(const key_type& x) const;
    template<class K> bool contains(const K& x) const;

    iterator       lower_bound(const key_type& x);
    const_iterator lower_bound(const key_type& x) const;
    template<class K> iterator       lower_bound(const K& x);
    template<class K> const_iterator lower_bound(const K& x) const;

    iterator       upper_bound(const key_type& x);
    const_iterator upper_bound(const key_type& x) const;
    template<class K> iterator       upper_bound(const K& x);
    template<class K> const_iterator upper_bound(const K& x) const;

    pair<iterator, iterator>               equal_range(const key_type& x);
    pair<const_iterator, const_iterator>   equal_range(const key_type& x) const;
    template<class K>
      pair<iterator, iterator>             equal_range(const K& x);
    template<class K>
      pair<const_iterator, const_iterator> equal_range(const K& x) const;
  };

  template<class InputIt,
           class Compare = less<__iter_value_type<InputIt>>,
           class Allocator = allocator<__iter_value_type<InputIt>>>
    set(InputIt, InputIt,
        Compare = Compare(), Allocator = Allocator())
      -> set<__iter_value_type<InputIt>, Compare, Allocator>;

  template<ranges::input_range R, class Compare = less<ranges::range_value_t<R>>,
           class Allocator = allocator<ranges::range_value_t<R>>>
    set(from_range_t, R&&, Compare = Compare(), Allocator = Allocator())
      -> set<ranges::range_value_t<R>, Compare, Allocator>;

  template<class Key, class Compare = less<Key>, class Allocator = allocator<Key>>
    set(initializer_list<Key>, Compare = Compare(), Allocator = Allocator())
      -> set<Key, Compare, Allocator>;

  template<class InputIt, class Allocator>
    set(InputIt, InputIt, Allocator)
      -> set<__iter_value_type<InputIt>,
             less<__iter_value_type<InputIt>>, Allocator>;

  template<ranges::input_range R, class Allocator>
    set(from_range_t, R&&, Allocator)
      -> set<ranges::range_value_t<R>, less<ranges::range_value_t<R>>, Allocator>;

  template<class Key, class Allocator>
    set(initializer_list<Key>, Allocator) -> set<Key, less<Key>, Allocator>;
}
```

#### Class template `std::multiset`

```cpp
namespace std {
  template<class Key, class Compare = less<Key>,
           class Allocator = allocator<Key>>
  class multiset {
  public:
    // types
    using key_type               = Key;
    using key_compare            = Compare;
    using value_type             = Key;
    using value_compare          = Compare;
    using allocator_type         = Allocator;
    using pointer                = typename allocator_traits<Allocator>::pointer;
    using const_pointer          = typename allocator_traits<Allocator>::const_pointer;
    using reference              = value_type&;
    using const_reference        = const value_type&;
    using size_type              = /* implementation-defined */;
    using difference_type        = /* implementation-defined */;
    using iterator               = /* implementation-defined */;
    using const_iterator         = /* implementation-defined */;
    using reverse_iterator       = std::reverse_iterator<iterator>;
    using const_reverse_iterator = std::reverse_iterator<const_iterator>;
    using node_type              = /* unspecified */;

    // construct/copy/destroy
    multiset() : multiset(Compare()) { }
    explicit multiset(const Compare& comp, const Allocator& = Allocator());
    template<class InputIt>
      multiset(InputIt first, InputIt last,
               const Compare& comp = Compare(), const Allocator& = Allocator());
    template<container-compatible-range<value_type> R>
      multiset(from_range_t, R&& rg,
               const Compare& comp = Compare(), const Allocator& = Allocator());
    multiset(const multiset& x);
    multiset(multiset&& x);
    explicit multiset(const Allocator&);
    multiset(const multiset&, const type_identity_t<Allocator>&);
    multiset(multiset&&, const type_identity_t<Allocator>&);
    multiset(initializer_list<value_type>, const Compare& = Compare(),
             const Allocator& = Allocator());
    template<class InputIt>
      multiset(InputIt first, InputIt last, const Allocator& a)
        : multiset(first, last, Compare(), a) { }
    template<container-compatible-range<value_type> R>
      multiset(from_range_t, R&& rg, const Allocator& a))
        : multiset(from_range, std::forward<R>(rg), Compare(), a) { }
    multiset(initializer_list<value_type> il, const Allocator& a)
      : multiset(il, Compare(), a) { }
    ~multiset();
    multiset& operator=(const multiset& x);
    multiset& operator=(multiset&& x)
      noexcept(allocator_traits<Allocator>::is_always_equal::value &&
               is_nothrow_move_assignable_v<Compare>);
    multiset& operator=(initializer_list<value_type>);
    allocator_type get_allocator() const noexcept;

    // iterators
    iterator               begin() noexcept;
    const_iterator         begin() const noexcept;
    iterator               end() noexcept;
    const_iterator         end() const noexcept;

    reverse_iterator       rbegin() noexcept;
    const_reverse_iterator rbegin() const noexcept;
    reverse_iterator       rend() noexcept;
    const_reverse_iterator rend() const noexcept;

    const_iterator         cbegin() const noexcept;
    const_iterator         cend() const noexcept;
    const_reverse_iterator crbegin() const noexcept;
    const_reverse_iterator crend() const noexcept;

    // capacity
    [[nodiscard]] bool empty() const noexcept;
    size_type size() const noexcept;
    size_type max_size() const noexcept;

    // modifiers
    template<class... Args> iterator emplace(Args&&... args);
    template<class... Args> iterator emplace_hint(const_iterator position,
                                                  Args&&... args);
    iterator insert(const value_type& x);
    iterator insert(value_type&& x);
    iterator insert(const_iterator position, const value_type& x);
    iterator insert(const_iterator position, value_type&& x);
    template<class InputIt>
      void insert(InputIt first, InputIt last);
    template<container-compatible-range<value_type> R>
      void insert_range(R&& rg);
    void insert(initializer_list<value_type>);

    node_type extract(const_iterator position);
    node_type extract(const key_type& x);
    template<class K> node_type extract(K&& x);
    iterator insert(node_type&& nh);
    iterator insert(const_iterator hint, node_type&& nh);

    iterator  erase(iterator position)
      requires (!same_as<iterator, const_iterator>);
    iterator  erase(const_iterator position);
    size_type erase(const key_type& x);
    template<class K> size_type erase(K&& x);
    iterator  erase(const_iterator first, const_iterator last);
    void      swap(multiset&)
      noexcept(allocator_traits<Allocator>::is_always_equal::value &&
               is_nothrow_swappable_v<Compare>);
    void      clear() noexcept;

    template<class C2>
      void merge(multiset<Key, C2, Allocator>& source);
    template<class C2>
      void merge(multiset<Key, C2, Allocator>&& source);
    template<class C2>
      void merge(set<Key, C2, Allocator>& source);
    template<class C2>
      void merge(set<Key, C2, Allocator>&& source);

    // observers
    key_compare key_comp() const;
    value_compare value_comp() const;

    // set operations
    iterator       find(const key_type& x);
    const_iterator find(const key_type& x) const;
    template<class K> iterator       find(const K& x);
    template<class K> const_iterator find(const K& x) const;

    size_type      count(const key_type& x) const;
    template<class K> size_type count(const K& x) const;

    bool           contains(const key_type& x) const;
    template<class K> bool contains(const K& x) const;

    iterator       lower_bound(const key_type& x);
    const_iterator lower_bound(const key_type& x) const;
    template<class K> iterator       lower_bound(const K& x);
    template<class K> const_iterator lower_bound(const K& x) const;

    iterator       upper_bound(const key_type& x);
    const_iterator upper_bound(const key_type& x) const;
    template<class K> iterator       upper_bound(const K& x);
    template<class K> const_iterator upper_bound(const K& x) const;

    pair<iterator, iterator>               equal_range(const key_type& x);
    pair<const_iterator, const_iterator>   equal_range(const key_type& x) const;
    template<class K>
      pair<iterator, iterator>             equal_range(const K& x);
    template<class K>
      pair<const_iterator, const_iterator> equal_range(const K& x) const;
  };

  template<class InputIt,
           class Compare = less<__iter_value_type<InputIt>>,
           class Allocator = allocator<__iter_value_type<InputIt>>>
    multiset(InputIt, InputIt,
             Compare = Compare(), Allocator = Allocator())
      -> multiset<__iter_value_type<InputIt>, Compare, Allocator>;

  template<ranges::input_range R, class Compare = less<ranges::range_value_t<R>>,
           class Allocator = allocator<ranges::range_value_t<R>>>
    multiset(from_range_t, R&&, Compare = Compare(), Allocator = Allocator())
      -> multiset<ranges::range_value_t<R>, Compare, Allocator>;

  template<class Key, class Compare = less<Key>, class Allocator = allocator<Key>>
    multiset(initializer_list<Key>, Compare = Compare(), Allocator = Allocator())
      -> multiset<Key, Compare, Allocator>;

  template<class InputIt, class Allocator>
    multiset(InputIt, InputIt, Allocator)
      -> multiset<__iter_value_type<InputIt>,
                  less<__iter_value_type<InputIt>>, Allocator>;

  template<ranges::input_range R, class Allocator>
    multiset(from_range_t, R&&, Allocator)
      -> multiset<ranges::range_value_t<R>, less<ranges::range_value_t<R>>, Allocator>;

  template<class Key, class Allocator>
    multiset(initializer_list<Key>, Allocator) -> multiset<Key, less<Key>, Allocator>;
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 214 | C++98 | the declarations of the members `find`, `lower_bound`,
      `upper_bound` and `equal_range` of `std::set` and `std::multiset` in the
      synopsis did not match the requirements on associative containers |
      corrected

---
*Source: https://en.cppreference.com/w/cpp/header/set*
