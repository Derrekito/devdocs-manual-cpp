# Standard library header <map>

This header is part of the containers library.

**Includes**

- **<compare> (C++20)** — Three-way comparison operator support
- **<initializer_list> (C++11)** — `std::initializer_list` class template

**Classes**

- **map** — collection of key-value pairs, sorted by keys, keys are unique
  (class template)
- **multimap** — collection of key-value pairs, sorted by keys (class template)

**Functions**

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values of
  two `maps` (function template)
- **std::swap(std::map)** — specializes the `std::swap` algorithm (function
  template)
- **erase_if(std::map) (C++20)** — erases all elements satisfying specific
  criteria (function template)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values of
  two `multimaps` (function template)
- **std::swap(std::multimap)** — specializes the `std::swap` algorithm (function
  template)
- **erase_if(std::multimap) (C++20)** — erases all elements satisfying specific
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
  // class template map
  template<class Key, class T, class Compare = less<Key>,
           class Allocator = allocator<pair<const Key, T>>>
    class map;

  template<class Key, class T, class Compare, class Allocator>
    bool operator==(const map<Key, T, Compare, Allocator>& x,
                    const map<Key, T, Compare, Allocator>& y);
  template<class Key, class T, class Compare, class Allocator>
    __synth_three_way_result<pair<const Key, T>>
      operator<=>(const map<Key, T, Compare, Allocator>& x,
                  const map<Key, T, Compare, Allocator>& y);

  template<class Key, class T, class Compare, class Allocator>
    void swap(map<Key, T, Compare, Allocator>& x,
              map<Key, T, Compare, Allocator>& y)
      noexcept(noexcept(x.swap(y)));

  // erasure for map
  template<class Key, class T, class Compare, class Allocator, class Predicate>
    typename map<Key, T, Compare, Allocator>::size_type
      erase_if(map<Key, T, Compare, Allocator>& c, Predicate pred);

  // class template multimap
  template<class Key, class T, class Compare = less<Key>,
           class Allocator = allocator<pair<const Key, T>>>
    class multimap;

  template<class Key, class T, class Compare, class Allocator>
    bool operator==(const multimap<Key, T, Compare, Allocator>& x,
                    const multimap<Key, T, Compare, Allocator>& y);
  template<class Key, class T, class Compare, class Allocator>
    __synth_three_way_result<pair<const Key, T>>
      operator<=>(const multimap<Key, T, Compare, Allocator>& x,
                  const multimap<Key, T, Compare, Allocator>& y);

  template<class Key, class T, class Compare, class Allocator>
    void swap(multimap<Key, T, Compare, Allocator>& x,
              multimap<Key, T, Compare, Allocator>& y)
      noexcept(noexcept(x.swap(y)));

  // erasure for multimap
  template<class Key, class T, class Compare, class Allocator, class Predicate>
    typename multimap<Key, T, Compare, Allocator>::size_type
      erase_if(multimap<Key, T, Compare, Allocator>& c, Predicate pred);

  namespace pmr {
    template<class Key, class T, class Compare = less<Key>>
      using map = std::map<Key, T, Compare,
                           polymorphic_allocator<pair<const Key, T>>>;

    template<class Key, class T, class Compare = less<Key>>
      using multimap = std::multimap<Key, T, Compare,
                                     polymorphic_allocator<pair<const Key, T>>>;
  }
}
```

#### Class template `std::map`

```cpp
namespace std {
  template<class Key, class T, class Compare = less<Key>,
           class Allocator = allocator<pair<const Key, T>>>
  class map {
  public:
    // types
    using key_type               = Key;
    using mapped_type            = T;
    using value_type             = pair<const Key, T>;
    using key_compare            = Compare;
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

    class value_compare {
      friend class map;
    protected:
      Compare comp;
      value_compare(Compare c) : comp(c) {}

    public:
      bool operator()(const value_type& x, const value_type& y) const {
        return comp(x.first, y.first);
      }
    };

    // construct/copy/destroy
    map() : map(Compare()) { }
    explicit map(const Compare& comp, const Allocator& = Allocator());
    template<class InputIt>
      map(InputIt first, InputIt last,
          const Compare& comp = Compare(), const Allocator& = Allocator());
    template<container-compatible-range<value_type> R>
      map(from_range_t, R&& rg, const Compare& comp = Compare(),
          const Allocator& = Allocator());
    map(const map& x);
    map(map&& x);
    explicit map(const Allocator&);
    map(const map&, const type_identity_t<Allocator>&);
    map(map&&, const type_identity_t<Allocator>&);
    map(initializer_list<value_type>,
      const Compare& = Compare(),
      const Allocator& = Allocator());
    template<class InputIt>
      map(InputIt first, InputIt last, const Allocator& a)
        : map(first, last, Compare(), a) { }
    template<container-compatible-range<value_type> R>
      map(from_range_t, R&& rg, const Allocator& a))
        : map(from_range, std::forward<R>(rg), Compare(), a) { }
    map(initializer_list<value_type> il, const Allocator& a)
      : map(il, Compare(), a) { }
    ~map();
    map& operator=(const map& x);
    map& operator=(map&& x)
      noexcept(allocator_traits<Allocator>::is_always_equal::value &&
               is_nothrow_move_assignable_v<Compare>);
    map& operator=(initializer_list<value_type>);
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

    // element access
    mapped_type& operator[](const key_type& x);
    mapped_type& operator[](key_type&& x);
    mapped_type&       at(const key_type& x);
    const mapped_type& at(const key_type& x) const;

    // modifiers
    template<class... Args> pair<iterator, bool> emplace(Args&&... args);
    template<class... Args> iterator emplace_hint(const_iterator position,
                                                  Args&&... args);
    pair<iterator, bool> insert(const value_type& x);
    pair<iterator, bool> insert(value_type&& x);
    template<class P> pair<iterator, bool> insert(P&& x);
    iterator insert(const_iterator position, const value_type& x);
    iterator insert(const_iterator position, value_type&& x);
    template<class P>
      iterator insert(const_iterator position, P&&);
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

    template<class... Args>
      pair<iterator, bool> try_emplace(const key_type& k, Args&&... args);
    template<class... Args>
      pair<iterator, bool> try_emplace(key_type&& k, Args&&... args);
    template<class... Args>
      iterator try_emplace(const_iterator hint, const key_type& k, Args&&... args);
    template<class... Args>
      iterator try_emplace(const_iterator hint, key_type&& k, Args&&... args);
    template<class M>
      pair<iterator, bool> insert_or_assign(const key_type& k, M&& obj);
    template<class M>
      pair<iterator, bool> insert_or_assign(key_type&& k, M&& obj);
    template<class M>
      iterator insert_or_assign(const_iterator hint, const key_type& k, M&& obj);
    template<class M>
      iterator insert_or_assign(const_iterator hint, key_type&& k, M&& obj);

    iterator  erase(iterator position);
    iterator  erase(const_iterator position);
    size_type erase(const key_type& x);
    template<class K> size_type erase(K&& x);
    iterator  erase(const_iterator first, const_iterator last);
    void      swap(map&)
      noexcept(allocator_traits<Allocator>::is_always_equal::value &&
               is_nothrow_swappable_v<Compare>);
    void      clear() noexcept;

    template<class C2>
      void merge(map<Key, T, C2, Allocator>& source);
    template<class C2>
      void merge(map<Key, T, C2, Allocator>&& source);
    template<class C2>
      void merge(multimap<Key, T, C2, Allocator>& source);
    template<class C2>
      void merge(multimap<Key, T, C2, Allocator>&& source);

    // observers
    key_compare key_comp() const;
    value_compare value_comp() const;

    // map operations
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

  template<class InputIt, class Compare = less<__iter_key_type<InputIt>>,
           class Allocator = allocator<__iter_to_alloc_type<InputIt>>>
    map(InputIt, InputIt, Compare = Compare(), Allocator = Allocator())
      -> map<__iter_key_type<InputIt>, __iter_mapped_type<InputIt>, Compare, Allocator>;

  template<ranges::input_range R, class Compare = less<__range_key_type<R>,
           class Allocator = allocator<__range_to_alloc_type<R>>>
    map(from_range_t, R&&, Compare = Compare(), Allocator = Allocator())
      -> map<__range_key_type<R>, __range_mapped_type<R>, Compare, Allocator>;

  template<class Key, class T, class Compare = less<Key>,
           class Allocator = allocator<pair<const Key, T>>>
    map(initializer_list<pair<Key, T>>, Compare = Compare(), Allocator = Allocator())
      -> map<Key, T, Compare, Allocator>;

  template<class InputIt, class Allocator>
    map(InputIt, InputIt, Allocator)
      -> map<__iter_key_type<InputIt>, __iter_mapped_type<InputIt>,
             less<__iter_key_type<InputIt>>, Allocator>;

  template<ranges::input_range R, class Allocator>
    map(from_range_t, R&&, Allocator)
      -> map<__range_key_type<R>, __range_mapped_type<R>, less<__range_key_type<R>>,
             Allocator>;

  template<class Key, class T, class Allocator>
    map(initializer_list<pair<Key, T>>, Allocator) -> map<Key, T, less<Key>, Allocator>;
}
```

#### Class template `std::multimap`

```cpp
namespace std {
  template<class Key, class T, class Compare = less<Key>,
           class Allocator = allocator<pair<const Key, T>>>
  class multimap {
  public:
    // types
    using key_type               = Key;
    using mapped_type            = T;
    using value_type             = pair<const Key, T>;
    using key_compare            = Compare;
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

    class value_compare {
      friend class multimap;
    protected:
      Compare comp;
      value_compare(Compare c) : comp(c) { }

    public:
      bool operator()(const value_type& x, const value_type& y) const {
        return comp(x.first, y.first);
      }
    };

    // construct/copy/destroy
    multimap() : multimap(Compare()) { }
    explicit multimap(const Compare& comp, const Allocator& = Allocator());
    template<class InputIt>
      multimap(InputIt first, InputIt last,
               const Compare& comp = Compare(),
               const Allocator& = Allocator());
    template<container-compatible-range<value_type> R>
      multimap(from_range_t, R&& rg,
               const Compare& comp = Compare(), const Allocator& = Allocator());
    multimap(const multimap& x);
    multimap(multimap&& x);
    explicit multimap(const Allocator&);
    multimap(const multimap&, const type_identity_t<Allocator>&);
    multimap(multimap&&, const type_identity_t<Allocator>&);
    multimap(initializer_list<value_type>,
      const Compare& = Compare(),
      const Allocator& = Allocator());
    template<class InputIt>
      multimap(InputIt first, InputIt last, const Allocator& a)
        : multimap(first, last, Compare(), a) { }
    template<container-compatible-range<value_type> R>
      multimap(from_range_t, R&& rg, const Allocator& a))
        : multimap(from_range, std::forward<R>(rg), Compare(), a) { }
    multimap(initializer_list<value_type> il, const Allocator& a)
      : multimap(il, Compare(), a) { }
    ~multimap();
    multimap& operator=(const multimap& x);
    multimap& operator=(multimap&& x)
      noexcept(allocator_traits<Allocator>::is_always_equal::value &&
               is_nothrow_move_assignable_v<Compare>);
    multimap& operator=(initializer_list<value_type>);
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
    template<class P> iterator insert(P&& x);
    iterator insert(const_iterator position, const value_type& x);
    iterator insert(const_iterator position, value_type&& x);
    template<class P> iterator insert(const_iterator position, P&& x);
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

    iterator  erase(iterator position);
    iterator  erase(const_iterator position);
    size_type erase(const key_type& x);
    template<class K> size_type erase(K&& x);
    iterator  erase(const_iterator first, const_iterator last);
    void      swap(multimap&)
      noexcept(allocator_traits<Allocator>::is_always_equal::value &&
               is_nothrow_swappable_v<Compare>);
    void      clear() noexcept;

    template<class C2>
      void merge(multimap<Key, T, C2, Allocator>& source);
    template<class C2>
      void merge(multimap<Key, T, C2, Allocator>&& source);
    template<class C2>
      void merge(map<Key, T, C2, Allocator>& source);
    template<class C2>
      void merge(map<Key, T, C2, Allocator>&& source);

    // observers
    key_compare key_comp() const;
    value_compare value_comp() const;

    // map operations
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

  template<class InputIt, class Compare = less<__iter_key_type<InputIt>>,
           class Allocator = allocator<__iter_to_alloc_type<InputIt>>>
    multimap(InputIt, InputIt, Compare = Compare(), Allocator = Allocator())
      -> multimap<__iter_key_type<InputIt>, __iter_mapped_type<InputIt>,
                  Compare, Allocator>;

  template<ranges::input_range R, class Compare = less<__range_key_type<R>>,
           class Allocator = allocator<__range_to_alloc_type<R>>>
    multimap(from_range_t, R&&, Compare = Compare(), Allocator = Allocator())
      -> multimap<__range_key_type<R>, __range_mapped_type<R>, Compare, Allocator>;

  template<class Key, class T, class Compare = less<Key>,
           class Allocator = allocator<pair<const Key, T>>>
    multimap(initializer_list<pair<Key, T>>, Compare = Compare(), Allocator = Allocator())
      -> multimap<Key, T, Compare, Allocator>;

  template<class InputIt, class Allocator>
    multimap(InputIt, InputIt, Allocator)
      -> multimap<__iter_key_type<InputIt>, __iter_mapped_type<InputIt>,
                  less<__iter_key_type<InputIt>>, Allocator>;

  template<ranges::input_range R, class Allocator>
    multimap(from_range_t, R&&, Allocator)
      -> multimap<__range_key_type<R>, __range_mapped_type<R>, less<__range_key_type<R>>,
                  Allocator>;

  template<class Key, class T, class Allocator>
    multimap(initializer_list<pair<Key, T>>, Allocator)
      -> multimap<Key, T, less<Key>, Allocator>;
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 133 | C++98 | `map::get_allocator` was missing in the synopsis | added

---
*Source: https://en.cppreference.com/w/cpp/header/map*
