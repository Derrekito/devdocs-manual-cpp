# Standard library header <flat_set> (C++23)

This header is part of the containers library.

**Includes**

- **<compare> (C++20)** — Three-way comparison operator support
- **<initializer_list> (C++11)** — `std::initializer_list` class template

**Classes**

- **flat_set (C++23)** — adapts a container to provide a collection of unique
  keys, sorted by keys (class template)
- **flat_multiset (C++23)** — adapts a container to provide a collection of
  keys, sorted by keys (class template)
- **std::uses_allocator<std::flat_set> (C++23)** — specializes the
  `std::uses_allocator` type trait (class template specialization)
- **std::uses_allocator<std::flat_multiset> (C++23)** — specializes the
  `std::uses_allocator` type trait (class template specialization)

**Functions**

- **erase_if(std::flat_set) (C++23)** — erases all elements satisfying specific
  criteria (function template)
- **erase_if(std::flat_multiset) (C++23)** — erases all elements satisfying
  specific criteria (function template)

**Tags**

- **sorted_uniquesorted_unique_t (C++23)** — a tag used to indicate that
  elements of a container or range are sorted and unique (tag)
- **sorted_equivalentsorted_equivalent_t (C++23)** — a tag used to indicate that
  elements of a container or range are sorted (uniqueness is not required) (tag)

### Synopsis

```cpp
#include <compare>
#include <initializer_list>

namespace std {
  // class template flat_set
  template<class Key, class Compare = less<Key>, class KeyContainer = vector<Key>>
    class flat_set;

  struct sorted_unique_t { explicit sorted_unique_t() = default; };
  inline constexpr sorted_unique_t sorted_unique{};

  template<class Key, class Compare, class KeyContainer, class Predicate>
    size_t erase_if(flat_set<Key, Compare, KeyContainer>& c, Predicate pred);

  template<class Key, class Compare, class KeyContainer, class Allocator>
    struct uses_allocator<flat_set<Key, Compare, KeyContainer>, Allocator>;

  // class template flat_multiset
  template<class Key, class Compare = less<Key>, class KeyContainer = vector<Key>>
    class flat_multiset;

  struct sorted_equivalent_t { explicit sorted_equivalent_t() = default; };
  inline constexpr sorted_equivalent_t sorted_equivalent{};

  template<class Key, class Compare, class KeyContainer, class Predicate>
    size_t erase_if(flat_multiset<Key, Compare, KeyContainer>& c, Predicate pred);

  template<class Key, class Compare, class KeyContainer, class Allocator>
    struct uses_allocator<flat_multiset<Key, Compare, KeyContainer>, Allocator>;
}
```

#### Class template `std::flat_set`

```cpp
namespace std {
  template<class Key, class Compare = less<Key>, class KeyContainer = vector<Key>>
  class flat_set {
  public:
    // types
    using key_type                  = Key;
    using value_type                = Key;
    using key_compare               = Compare;
    using value_compare             = Compare;
    using reference                 = value_type&;
    using const_reference           = const value_type&;
    using size_type                 = typename KeyContainer::size_type;
    using difference_type           = typename KeyContainer::difference_type;
    using iterator                  = /* implementation-defined */;
    using const_iterator            = /* implementation-defined */;
    using reverse_iterator          = std::reverse_iterator<iterator>;
    using const_reverse_iterator    = std::reverse_iterator<const_iterator>;
    using container_type            = KeyContainer;

    // constructors
    flat_set() : flat_set(key_compare()) { }

    explicit flat_set(container_type cont);
    template<class Allocator>
      flat_set(const container_type& cont, const Allocator& a);

    flat_set(sorted_unique_t, container_type cont)
      : c(std::move(cont)), compare(key_compare()) { }
    template<class Allocator>
      flat_set(sorted_unique_t, const container_type& cont, const Allocator& a);

    explicit flat_set(const key_compare& comp)
      : c(), compare(comp) { }
    template<class Allocator>
      flat_set(const key_compare& comp, const Allocator& a);
    template<class Allocator>
      explicit flat_set(const Allocator& a);

    template<class InputIterator>
      flat_set(InputIterator first, InputIterator last,
               const key_compare& comp = key_compare())
        : c(), compare(comp)
        { insert(first, last); }
    template<class InputIterator, class Allocator>
      flat_set(InputIterator first, InputIterator last,
               const key_compare& comp, const Allocator& a);
    template<class InputIterator, class Allocator>
      flat_set(InputIterator first, InputIterator last, const Allocator& a);

    template<container-compatible-range<value_type> R>
      flat_set(from_range_t fr, R&& rg)
        : flat_set(fr, std::forward<R>(range), key_compare()) { }
    template<container-compatible-range<value_type> R, class Allocator>
      flat_set(from_range_t, R&& rg, const Allocator& a);
    template<container-compatible-range<value_type> R>
      flat_set(from_range_t, R&& rg, const key_compare& comp)
        : flat_set(comp)
        { insert_range(std::forward<R>(range)); }
    template<container-compatible-range<value_type> R, class Allocator>
       flat_set(from_range_t, R&& rg, const key_compare& comp,
                const Allocator& a);

    template<class InputIterator>
      flat_set(sorted_unique_t, InputIterator first, InputIterator last,
               const key_compare& comp = key_compare())
        : c(first, last), compare(comp) { }
    template<class InputIterator, class Allocator>
      flat_set(sorted_unique_t, InputIterator first, InputIterator last,
               const key_compare& comp, const Allocator& a);
    template<class InputIterator, class Allocator>
      flat_set(sorted_unique_t, InputIterator first, InputIterator last,
               const Allocator& a);

    flat_set(initializer_list<key_type> il,
             const key_compare& comp = key_compare())
        : flat_set(il.begin(), il.end(), comp) { }
    template<class Allocator>
      flat_set(initializer_list<key_type> il,
               const key_compare& comp, const Allocator& a);
    template<class Allocator>
      flat_set(initializer_list<key_type> il, const Allocator& a);

    flat_set(sorted_unique_t s, initializer_list<key_type> il,
             const key_compare& comp = key_compare())
        : flat_set(s, il.begin(), il.end(), comp) { }
    template<class Allocator>
      flat_set(sorted_unique_t, initializer_list<key_type> il,
               const key_compare& comp, const Allocator& a);
    template<class Allocator>
      flat_set(sorted_unique_t, initializer_list<key_type> il,
               const Allocator& a);

    flat_set& operator=(initializer_list<key_type>);

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
    template<class... Args>
      iterator emplace_hint(const_iterator position, Args&&... args);

    pair<iterator, bool> insert(const value_type& x)
      { return emplace(x); }
    pair<iterator, bool> insert(value_type&& x)
      { return emplace(std::move(x)); }
    template<class K> pair<iterator, bool> insert(K&& x);
    iterator insert(const_iterator position, const value_type& x)
      { return emplace_hint(position, x); }
    iterator insert(const_iterator position, value_type&& x)
      { return emplace_hint(position, std::move(x)); }
    template<class K> iterator insert(const_iterator hint, K&& x);

    template<class InputIterator>
      void insert(InputIterator first, InputIterator last);
    template<class InputIterator>
      void insert(sorted_unique_t, InputIterator first, InputIterator last);
    template<container-compatible-range<value_type> R>
      void insert_range(R&& rg);

    void insert(initializer_list<key_type> il)
      { insert(il.begin(), il.end()); }
    void insert(sorted_unique_t s, initializer_list<key_type> il)
      { insert(s, il.begin(), il.end()); }

    container_type extract() &&;
    void replace(container_type&&);

    iterator erase(iterator position);
    iterator erase(const_iterator position);
    size_type erase(const key_type& x);
    template<class K> size_type erase(K&& x);
    iterator erase(const_iterator first, const_iterator last);

    void swap(flat_set& y) noexcept;
    void clear() noexcept;

    // observers
    key_compare key_comp() const;
    value_compare value_comp() const;

    // set operations
    iterator find(const key_type& x);
    const_iterator find(const key_type& x) const;
    template<class K> iterator find(const K& x);
    template<class K> const_iterator find(const K& x) const;

    size_type count(const key_type& x) const;
    template<class K> size_type count(const K& x) const;

    bool contains(const key_type& x) const;
    template<class K> bool contains(const K& x) const;

    iterator lower_bound(const key_type& x);
    const_iterator lower_bound(const key_type& x) const;
    template<class K> iterator lower_bound(const K& x);
    template<class K> const_iterator lower_bound(const K& x) const;

    iterator upper_bound(const key_type& x);
    const_iterator upper_bound(const key_type& x) const;
    template<class K> iterator upper_bound(const K& x);
    template<class K> const_iterator upper_bound(const K& x) const;

    pair<iterator, iterator> equal_range(const key_type& x);
    pair<const_iterator, const_iterator> equal_range(const key_type& x) const;
    template<class K>
      pair<iterator, iterator> equal_range(const K& x);
    template<class K>
      pair<const_iterator, const_iterator> equal_range(const K& x) const;

    friend bool operator==(const flat_set& x, const flat_set& y);

    friend /*synth-three-way-result*/<value_type>
      operator<=>(const flat_set& x, const flat_set& y);

    friend void swap(flat_set& x, flat_set& y) noexcept { x.swap(y); }

  private:
    container_type c;           // exposition only
    key_compare compare;        // exposition only
  };

  template<class InputIterator, class Compare = less</*iter-value-type*/<InputIterator>>>
    flat_set(InputIterator, InputIterator, Compare = Compare())
      -> flat_set</*iter-value-type*/<InputIterator>, Compare>;

  template<class InputIterator, class Compare = less</*iter-value-type*/<InputIterator>>>
    flat_set(sorted_unique_t, InputIterator, InputIterator, Compare = Compare())
      -> flat_set</*iter-value-type*/<InputIterator>, Compare>;

  template<ranges::input_range R, class Compare = less<ranges::range_value_t<R>>,
          class Allocator = allocator<ranges::range_value_t<R>>>
    flat_set(from_range_t, R&&, Compare = Compare(), Allocator = Allocator())
      -> flat_set<ranges::range_value_t<R>, Compare>;

   template<ranges::input_range R, class Allocator>
     flat_set(from_range_t, R&&, Allocator)
       -> flat_set<ranges::range_value_t<R>, less<ranges::range_value_t<R>>>;

  template<class Key, class Compare = less<Key>>
    flat_set(initializer_list<Key>, Compare = Compare())
      -> flat_set<Key, Compare>;

  template<class Key, class Compare = less<Key>>
    flat_set(sorted_unique_t, initializer_list<Key>, Compare = Compare())
      -> flat_set<Key, Compare>;

  template<class Key, class Compare, class KeyContainer, class Allocator>
    struct uses_allocator<flat_set<Key, Compare, KeyContainer>, Allocator>
      : bool_constant<uses_allocator_v<KeyContainer, Allocator>> { };
}
```

#### Class template `std::flat_multiset`

```cpp
namespace std {
  template<class Key, class Compare = less<Key>, class KeyContainer = vector<Key>>
  class flat_multiset {
  public:
    // types
    using key_type                  = Key;
    using value_type                = Key;
    using key_compare               = Compare;
    using value_compare             = Compare;
    using reference                 = value_type&;
    using const_reference           = const value_type&;
    using size_type                 = typename KeyContainer::size_type;
    using difference_type           = typename KeyContainer::difference_type;
    using iterator                  = /* implementation-defined */;
    using const_iterator            = /* implementation-defined */;
    using reverse_iterator          = std::reverse_iterator<iterator>;
    using const_reverse_iterator    = std::reverse_iterator<const_iterator>;
    using container_type            = KeyContainer;

    // constructors
    flat_multiset() : flat_multiset(key_compare()) { }

    explicit flat_multiset(container_type cont);
    template<class Allocator>
      flat_multiset(const container_type& cont, const Allocator& a);

    flat_multiset(sorted_equivalent_t, container_type cont)
      : c(std::move(cont)), compare(key_compare()) { }
    template<class Allocator>
      flat_multiset(sorted_equivalent_t, const container_type&, const Allocator& a);

    explicit flat_multiset(const key_compare& comp)
      : c(), compare(comp) { }
    template<class Allocator>
      flat_multiset(const key_compare& comp, const Allocator& a);
    template<class Allocator>
      explicit flat_multiset(const Allocator& a);

    template<class InputIterator>
      flat_multiset(InputIterator first, InputIterator last,
                    const key_compare& comp = key_compare())
        : c(), compare(comp)
        { insert(first, last); }
    template<class InputIterator, class Allocator>
      flat_multiset(InputIterator first, InputIterator last,
                    const key_compare& comp, const Allocator& a);
    template<class InputIterator, class Allocator>
      flat_multiset(InputIterator first, InputIterator last,
                    const Allocator& a);

    template<container-compatible-range<value_type> R>
      flat_multiset(from_range_t fr, R&& rg)
        : flat_multiset(fr, std::forward<R>(range), key_compare()) { }
    template<container-compatible-range<value_type> R, class Allocator>
      flat_multiset(from_range_t, R&& rg, const Allocator& a);
    template<container-compatible-range<value_type> R>
      flat_multiset(from_range_t, R&& rg, const key_compare& comp)
        : flat_multiset(comp)
        { insert_range(std::forward<R>(range)); }
    template<container-compatible-range<value_type> R, class Allocator>
      flat_multiset(from_range_t, R&& rg, const key_compare& comp,
                    const Allocator& a);

    template<class InputIterator>
      flat_multiset(sorted_equivalent_t, InputIterator first, InputIterator last,
                    const key_compare& comp = key_compare())
        : c(first, last), compare(comp) { }
    template<class InputIterator, class Allocator>
      flat_multiset(sorted_equivalent_t, InputIterator first, InputIterator last,
                    const key_compare& comp, const Allocator& a);
    template<class InputIterator, class Allocator>
      flat_multiset(sorted_equivalent_t, InputIterator first, InputIterator last,
                    const Allocator& a);

    flat_multiset(initializer_list<key_type> il,
                  const key_compare& comp = key_compare())
      : flat_multiset(il.begin(), il.end(), comp) { }
    template<class Allocator>
      flat_multiset(initializer_list<key_type> il,
                    const key_compare& comp, const Allocator& a);
    template<class Allocator>
      flat_multiset(initializer_list<key_type> il, const Allocator& a);

    flat_multiset(sorted_equivalent_t s, initializer_list<key_type> il,
                  const key_compare& comp = key_compare())
        : flat_multiset(s, il.begin(), il.end(), comp) { }
    template<class Allocator>
      flat_multiset(sorted_equivalent_t, initializer_list<key_type> il,
                    const key_compare& comp, const Allocator& a);
    template<class Allocator>
      flat_multiset(sorted_equivalent_t, initializer_list<key_type> il,
                    const Allocator& a);

    flat_multiset& operator=(initializer_list<key_type>);

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
    template<class... Args>
      iterator emplace_hint(const_iterator position, Args&&... args);

    iterator insert(const value_type& x)
      { return emplace(x); }
    iterator insert(value_type&& x)
      { return emplace(std::move(x)); }
    iterator insert(const_iterator position, const value_type& x)
      { return emplace_hint(position, x); }
    iterator insert(const_iterator position, value_type&& x)
      { return emplace_hint(position, std::move(x)); }

    template<class InputIterator>
      void insert(InputIterator first, InputIterator last);
    template<class InputIterator>
      void insert(sorted_equivalent_t, InputIterator first, InputIterator last);
    template<container-compatible-range<value_type> R>
      void insert_range(R&& rg);

    void insert(initializer_list<key_type> il)
      { insert(il.begin(), il.end()); }
    void insert(sorted_equivalent_t s, initializer_list<key_type> il)
      { insert(s, il.begin(), il.end()); }

    container_type extract() &&;
    void replace(container_type&&);

    iterator erase(iterator position);
    iterator erase(const_iterator position);
    size_type erase(const key_type& x);
    template<class K> size_type erase(K&& x);
    iterator erase(const_iterator first, const_iterator last);

    void swap(flat_multiset& y) noexcept;
    void clear() noexcept;

    // observers
    key_compare key_comp() const;
    value_compare value_comp() const;

    // set operations
    iterator find(const key_type& x);
    const_iterator find(const key_type& x) const;
    template<class K> iterator find(const K& x);
    template<class K> const_iterator find(const K& x) const;

    size_type count(const key_type& x) const;
    template<class K> size_type count(const K& x) const;

    bool contains(const key_type& x) const;
    template<class K> bool contains(const K& x) const;

    iterator lower_bound(const key_type& x);
    const_iterator lower_bound(const key_type& x) const;
    template<class K> iterator lower_bound(const K& x);
    template<class K> const_iterator lower_bound(const K& x) const;

    iterator upper_bound(const key_type& x);
    const_iterator upper_bound(const key_type& x) const;
    template<class K> iterator upper_bound(const K& x);
    template<class K> const_iterator upper_bound(const K& x) const;

    pair<iterator, iterator> equal_range(const key_type& x);
    pair<const_iterator, const_iterator> equal_range(const key_type& x) const;
    template<class K>
      pair<iterator, iterator> equal_range(const K& x);
    template<class K>
      pair<const_iterator, const_iterator> equal_range(const K& x) const;

    friend bool operator==(const flat_multiset& x, const flat_multiset& y);

    friend /*synth-three-way-result*/<value_type>
      operator<=>(const flat_multiset& x, const flat_multiset& y);

    friend void swap(flat_multiset& x, flat_multiset& y) noexcept
      { x.swap(y); }

  private:
    container_type c;       // exposition only
    key_compare compare;    // exposition only
  };

  template<class InputIterator, class Compare = less</*iter-value-type*/<InputIterator>>>
    flat_multiset(InputIterator, InputIterator, Compare = Compare())
      -> flat_multiset</*iter-value-type*/<InputIterator>,
                       /*iter-value-type*/<InputIterator>,
                       Compare>;

  template<class InputIterator, class Compare = less</*iter-value-type*/<InputIterator>>>
    flat_multiset(sorted_equivalent_t, InputIterator, InputIterator, Compare = Compare())
      -> flat_multiset</*iter-value-type*/<InputIterator>,
                       /*iter-value-type*/<InputIterator>, Compare>;

  template<ranges::input_range R, class Compare = less<ranges::range_value_t<R>>,
           class Allocator = allocator<ranges::range_value_t<R>>>
    flat_multiset(from_range_t, R&&, Compare = Compare(), Allocator = Allocator())
      -> flat_multiset<ranges::range_value_t<R>, Compare>;

   template<ranges::input_range R, class Allocator>
     flat_multiset(from_range_t, R&&, Allocator)
       -> flat_multiset<ranges::range_value_t<R>, less<ranges::range_value_t<R>>>;

  template<class Key, class Compare = less<Key>>
    flat_multiset(initializer_list<Key>, Compare = Compare())
      -> flat_multiset<Key, Compare>;

  template<class Key, class Compare = less<Key>>
  flat_multiset(sorted_equivalent_t, initializer_list<Key>, Compare = Compare())
      -> flat_multiset<Key, Compare>;

  template<class Key, class Compare, class KeyContainer, class Allocator>
    struct uses_allocator<flat_multiset<Key, Compare, KeyContainer>, Allocator>
      : bool_constant<uses_allocator_v<KeyContainer, Allocator>> { };
}
```

### References

- C++23 standard (ISO/IEC 14882:2023):

---
*Source: https://en.cppreference.com/w/cpp/header/flat_set*
