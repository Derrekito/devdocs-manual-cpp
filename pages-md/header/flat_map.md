# Standard library header <flat_map> (C++23)

This header is part of the containers library.

**Includes**

- **<compare> (C++20)** — Three-way comparison operator support
- **<initializer_list> (C++11)** — `std::initializer_list` class template

**Classes**

- **flat_map (C++23)** — adapts two containers to provide a collection of
  key-value pairs, sorted by unique keys (class template)
- **flat_multimap (C++23)** — adapts two containers to provide a collection of
  key-value pairs, sorted by keys (class template)
- **std::uses_allocator<std::flat_map> (C++23)** — specializes the
  `std::uses_allocator` type trait (class template specialization)
- **std::uses_allocator<std::flat_multimap> (C++23)** — specializes the
  `std::uses_allocator` type trait (class template specialization)

**Functions**

- **erase_if(std::flat_map) (C++23)** — erases all elements satisfying specific
  criteria (function template)
- **erase_if(std::flat_multimap) (C++23)** — erases all elements satisfying
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
  // class template flat_map
  template<class Key, class T, class Compare = less<Key>,
           class KeyContainer = vector<Key>, class MappedContainer = vector<T>>
    class flat_map;

  struct sorted_unique_t { explicit sorted_unique_t() = default; };
  inline constexpr sorted_unique_t sorted_unique{};

  template<class Key, class T, class Compare, class KeyContainer, class MappedContainer,
           class Predicate>
    size_t erase_if(flat_map<Key, T, Compare, KeyContainer, MappedContainer>& c,
                    Predicate pred);

  template<class Key, class T, class Compare, class KeyContainer, class MappedContainer,
           class Allocator>
    struct uses_allocator<flat_map<Key, T, Compare, KeyContainer, MappedContainer>,
                          Allocator>;

  // class template flat_multimap
  template<class Key, class T, class Compare = less<Key>,
           class KeyContainer = vector<Key>, class MappedContainer = vector<T>>
    class flat_multimap;

  struct sorted_equivalent_t { explicit sorted_equivalent_t() = default; };
  inline constexpr sorted_equivalent_t sorted_equivalent{};

  template<class Key, class T, class Compare, class KeyContainer, class MappedContainer,
           class Predicate>
    size_t erase_if(flat_multimap<Key, T, Compare, KeyContainer, MappedContainer>& c,
                    Predicate pred);

  template<class Key, class T, class Compare, class KeyContainer, class MappedContainer,
           class Allocator>
    struct uses_allocator<flat_multimap<Key, T, Compare, KeyContainer, MappedContainer>,
                          Allocator>;
}
```

#### Class template `std::flat_map`

```cpp
namespace std {
  template<class Key, class T, class Compare = less<Key>,
           class KeyContainer = vector<Key>, class MappedContainer = vector<T>>
  class flat_map {
  public:
    // types
    using key_type               = Key;
    using mapped_type            = T;
    using value_type             = pair<key_type, mapped_type>;
    using key_compare            = Compare;
    using reference              = pair<const key_type&, mapped_type&>;
    using const_reference        = pair<const key_type&, const mapped_type&>;
    using size_type              = size_t;
    using difference_type        = ptrdiff_t;
    using iterator               = /* implementation-defined */;
    using const_iterator         = /* implementation-defined */;
    using reverse_iterator       = std::reverse_iterator<iterator>;
    using const_reverse_iterator = std::reverse_iterator<const_iterator>;
    using key_container_type     = KeyContainer;
    using mapped_container_type  = MappedContainer;

    class value_compare {
    private:
      key_compare comp;                                 // exposition only
      value_compare(key_compare c) : comp(c) { }        // exposition only
    public:
      bool operator()(const_reference x, const_reference y) const {
        return comp(x.first, y.first);
      }
    };

    struct containers {
      key_container_type keys;
      mapped_container_type values;
    };

    // construct/copy/destroy
    flat_map() : flat_map(key_compare()) { }

    flat_map(key_container_type key_cont, mapped_container_type mapped_cont);
    template<class Allocator>
      flat_map(const key_container_type& key_cont,
               const mapped_container_type& mapped_cont,
               const Allocator& a);

    flat_map(sorted_unique_t, key_container_type key_cont,
             mapped_container_type mapped_cont);
    template<class Allocator>
      flat_map(sorted_unique_t, const key_container_type& key_cont,
               const mapped_container_type& mapped_cont, const Allocator& a);

    explicit flat_map(const key_compare& comp)
      : c(), compare(comp) { }
    template<class Allocator>
      flat_map(const key_compare& comp, const Allocator& a);
    template<class Allocator>
      explicit flat_map(const Allocator& a);

    template<class InputIterator>
      flat_map(InputIterator first, InputIterator last,
               const key_compare& comp = key_compare())
        : c(), compare(comp) { insert(first, last); }
    template<class InputIterator, class Allocator>
      flat_map(InputIterator first, InputIterator last,
               const key_compare& comp, const Allocator& a);
    template<class InputIterator, class Allocator>
      flat_map(InputIterator first, InputIterator last, const Allocator& a);

    template<container-compatible-range<value_type> R>
      flat_map(from_range_t fr, R&& rg)
        : flat_map(fr, std::forward<R>(rg), key_compare()) { }
    template<container-compatible-range<value_type> R, class Allocator>
      flat_map(from_range_t, R&& rg, const Allocator& a);
    template<container-compatible-range<value_type> R>
      flat_map(from_range_t, R&& rg, const key_compare& comp)
        : flat_map(comp) { insert_range(std::forward<R>(rg)); }
    template<container-compatible-range<value_type> R, class Allocator>
      flat_map(from_range_t, R&& rg, const key_compare& comp, const Allocator& a);

    template<class InputIterator>
      flat_map(sorted_unique_t s, InputIterator first, InputIterator last,
               const key_compare& comp = key_compare())
        : c(), compare(comp) { insert(s, first, last); }
    template<class InputIterator, class Allocator>
      flat_map(sorted_unique_t, InputIterator first, InputIterator last,
               const key_compare& comp, const Allocator& a);
    template<class InputIterator, class Allocator>
      flat_map(sorted_unique_t, InputIterator first, InputIterator last,
               const Allocator& a);

    flat_map(initializer_list<value_type> il, const key_compare& comp = key_compare())
        : flat_map(il.begin(), il.end(), comp) { }
    template<class Allocator>
      flat_map(initializer_list<value_type> il, const key_compare& comp,
               const Allocator& a);
    template<class Allocator>
      flat_map(initializer_list<value_type> il, const Allocator& a);

    flat_map(sorted_unique_t s, initializer_list<value_type> il,
             const key_compare& comp = key_compare())
        : flat_map(s, il.begin(), il.end(), comp) { }
    template<class Allocator>
      flat_map(sorted_unique_t, initializer_list<value_type> il,
               const key_compare& comp, const Allocator& a);
    template<class Allocator>
      flat_map(sorted_unique_t, initializer_list<value_type> il, const Allocator& a);

    flat_map& operator=(initializer_list<value_type> il);

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
    template<class K> mapped_type& operator[](K&& x);
    mapped_type& at(const key_type& x);
    const mapped_type& at(const key_type& x) const;
    template<class K> mapped_type& at(const K& x);
    template<class K> const mapped_type& at(const K& x) const;

    // modifiers
    template<class... Args> pair<iterator, bool> emplace(Args&&... args);
    template<class... Args>
      iterator emplace_hint(const_iterator position, Args&&... args);

    pair<iterator, bool> insert(const value_type& x)
      { return emplace(x); }
    pair<iterator, bool> insert(value_type&& x)
      { return emplace(std::move(x)); }
    iterator insert(const_iterator position, const value_type& x)
      { return emplace_hint(position, x); }
    iterator insert(const_iterator position, value_type&& x)
      { return emplace_hint(position, std::move(x)); }

    template<class P> pair<iterator, bool> insert(P&& x);
    template<class P>
      iterator insert(const_iterator position, P&&);
    template<class InputIterator>
      void insert(InputIterator first, InputIterator last);
    template<class InputIterator>
      void insert(sorted_unique_t, InputIterator first, InputIterator last);
    template<container-compatible-range<value_type> R>
      void insert_range(R&& rg);

    void insert(initializer_list<value_type> il)
      { insert(il.begin(), il.end()); }
    void insert(sorted_unique_t s, initializer_list<value_type> il)
      { insert(s, il.begin(), il.end()); }

    containers extract() &&;
    void replace(key_container_type&& key_cont, mapped_container_type&& mapped_cont);

    template<class... Args>
      pair<iterator, bool> try_emplace(const key_type& k, Args&&... args);
    template<class... Args>
      pair<iterator, bool> try_emplace(key_type&& k, Args&&... args);
    template<class K, class... Args>
      pair<iterator, bool> try_emplace(K&& k, Args&&... args);
    template<class... Args>
      iterator try_emplace(const_iterator hint, const key_type& k, Args&&... args);
    template<class... Args>
      iterator try_emplace(const_iterator hint, key_type&& k, Args&&... args);
    template<class K, class... Args>
      iterator try_emplace(const_iterator hint, K&& k, Args&&... args);
    template<class M>
      pair<iterator, bool> insert_or_assign(const key_type& k, M&& obj);
    template<class M>
      pair<iterator, bool> insert_or_assign(key_type&& k, M&& obj);
    template<class K, class M>
      pair<iterator, bool> insert_or_assign(K&& k, M&& obj);
    template<class M>
      iterator insert_or_assign(const_iterator hint, const key_type& k, M&& obj);
    template<class M>
      iterator insert_or_assign(const_iterator hint, key_type&& k, M&& obj);
    template<class K, class M>
      iterator insert_or_assign(const_iterator hint, K&& k, M&& obj);

    iterator erase(iterator position);
    iterator erase(const_iterator position);
    size_type erase(const key_type& x);
    template<class K> size_type erase(K&& x);
    iterator erase(const_iterator first, const_iterator last);

    void swap(flat_map& y) noexcept;
    void clear() noexcept;

    // observers
    key_compare key_comp() const;
    value_compare value_comp() const;

    const key_container_type& keys() const noexcept      { return c.keys; }
    const mapped_container_type& values() const noexcept { return c.values; }

    // map operations
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
    template<class K> pair<iterator, iterator> equal_range(const K& x);
    template<class K> pair<const_iterator, const_iterator> equal_range(const K& x) const;

    friend bool operator==(const flat_map& x, const flat_map& y);

    friend /*synth-three-way-result*/<value_type>
      operator<=>(const flat_map& x, const flat_map& y);

    friend void swap(flat_map& x, flat_map& y) noexcept
      { x.swap(y); }

  private:
    containers c;               // exposition only
    key_compare compare;        // exposition only

    struct key_equiv {  // exposition only
      key_equiv(key_compare c) : comp(c) { }
      bool operator()(const_reference x, const_reference y) const {
        return !comp(x.first, y.first) && !comp(y.first, x.first);
      }
      key_compare comp;
    };
  };

  template<class KeyContainer, class MappedContainer>
    flat_map(KeyContainer, MappedContainer)
      -> flat_map<typename KeyContainer::value_type, typename MappedContainer::value_type,
                  less<typename KeyContainer::value_type>, KeyContainer, MappedContainer>;

  template<class KeyContainer, class MappedContainer, class Allocator>
    flat_map(KeyContainer, MappedContainer, Allocator)
      -> flat_map<typename KeyContainer::value_type, typename MappedContainer::value_type,
                  less<typename KeyContainer::value_type>, KeyContainer, MappedContainer>;

  template<class KeyContainer, class MappedContainer>
    flat_map(sorted_unique_t, KeyContainer, MappedContainer)
      -> flat_map<typename KeyContainer::value_type, typename MappedContainer::value_type,
                  less<typename KeyContainer::value_type>, KeyContainer, MappedContainer>;

  template<class KeyContainer, class MappedContainer, class Allocator>
    flat_map(sorted_unique_t, KeyContainer, MappedContainer, Allocator)
      -> flat_map<typename KeyContainer::value_type, typename MappedContainer::value_type,
                  less<typename KeyContainer::value_type>, KeyContainer, MappedContainer>;

  template<class InputIterator, class Compare = less</*iter-key-type*/<InputIterator>>>
    flat_map(InputIterator, InputIterator, Compare = Compare())
      -> flat_map</*iter-key-type*/<InputIterator>, /*iter-mapped-type*/<InputIterator>,
                  Compare>;

  template<class InputIterator, class Compare = less</*iter-key-type*/<InputIterator>>>
    flat_map(sorted_unique_t, InputIterator, InputIterator, Compare = Compare())
      -> flat_map</*iter-key-type*/<InputIterator>, /*iter-mapped-type*/<InputIterator>,
                  Compare>;

  template<ranges::input_range R, class Compare = less</*range-key-type*/<R>>,
           class Allocator>
    flat_map(from_range_t, R&&, Compare = Compare(), Allocator = Allocator())
      -> flat_map</*range-key-type*/<R>, /*range-mapped-type*/<R>, Compare>;

  template<ranges::input_range R, class Allocator>
    flat_map(from_range_t, R&&, Allocator)
      -> flat_map</*range-key-type*/<R>, /*range-mapped-type*/<R>>;

  template<class Key, class T, class Compare = less<Key>>
    flat_map(initializer_list<pair<Key, T>>, Compare = Compare())
      -> flat_map<Key, T, Compare>;

  template<class Key, class T, class Compare = less<Key>>
    flat_map(sorted_unique_t, initializer_list<pair<Key, T>>, Compare = Compare())
        -> flat_map<Key, T, Compare>;

  template<class Key, class T, class Compare, class KeyContainer, class MappedContainer,
            class Allocator>
    struct uses_allocator<flat_map<Key, T, Compare, KeyContainer, MappedContainer>,
                          Allocator>
      : bool_constant<uses_allocator_v<KeyContainer, Allocator> &&
                      uses_allocator_v<MappedContainer, Allocator>> { };
}
```

#### Class template `std::flat_multimap`

```cpp
namespace std {
  template<class Key, class T, class Compare = less<Key>,
           class KeyContainer = vector<Key>, class MappedContainer = vector<T>>
  class flat_multimap {
  public:
    // types
    using key_type               = Key;
    using mapped_type            = T;
    using value_type             = pair<key_type, mapped_type>;
    using key_compare            = Compare;
    using reference              = pair<const key_type&, mapped_type&>;
    using const_reference        = pair<const key_type&, const mapped_type&>;
    using size_type              = size_t;
    using difference_type        = ptrdiff_t;
    using iterator               = /* implementation-defined */;
    using const_iterator         = /* implementation-defined */;
    using reverse_iterator       = std::reverse_iterator<iterator>;
    using const_reverse_iterator = std::reverse_iterator<const_iterator>;
    using key_container_type     = KeyContainer;
    using mapped_container_type  = MappedContainer;

    class value_compare {
    private:
      key_compare comp;                                 // exposition only
      value_compare(key_compare c) : comp(c) { }        // exposition only
    public:
      bool operator()(const_reference x, const_reference y) const {
        return comp(x.first, y.first);
      }
    };

    struct containers {
      key_container_type keys;
      mapped_container_type values;
    };

    // construct/copy/destroy
    flat_multimap() : flat_multimap(key_compare()) { }

    flat_multimap(key_container_type key_cont, mapped_container_type mapped_cont);
    template<class Allocator>
      flat_multimap(const key_container_type& key_cont,
                    const mapped_container_type& mapped_cont,
                    const Allocator& a);

    flat_multimap(sorted_equivalent_t,
                  key_container_type key_cont, mapped_container_type mapped_cont);
    template<class Allocator>
      flat_multimap(sorted_equivalent_t, const key_container_type& key_cont,
                    const mapped_container_type& mapped_cont, const Allocator& a);

    explicit flat_multimap(const key_compare& comp)
      : c(), compare(comp) { }
    template<class Allocator>
      flat_multimap(const key_compare& comp, const Allocator& a);
    template<class Allocator>
      explicit flat_multimap(const Allocator& a);

    template<class InputIterator>
      flat_multimap(InputIterator first, InputIterator last,
                    const key_compare& comp = key_compare())
        : c(), compare(comp)
        { insert(first, last); }
    template<class InputIterator, class Allocator>
      flat_multimap(InputIterator first, InputIterator last,
                    const key_compare& comp, const Allocator& a);
    template<class InputIterator, class Allocator>
      flat_multimap(InputIterator first, InputIterator last,
                    const Allocator& a);

    template<container-compatible-range<value_type> R>
      flat_mutlimap(from_range_t fr, R&& rg)
        : flat_multimap(fr, std::forward<R>(rg), key_compare()) { }
    template<container-compatible-range<value_type> R, class Allocator>
      flat_mutlimap(from_range_t, R&& rg, const Allocator& a);
    template<container-compatible-range<value_type> R>
      flat_multimap(from_range_t, R&& rg, const key_compare& comp)
        : flat_multimap(comp) { insert_range(std::forward<R>(rg)); }
    template<container-compatible-range<value_type> R, class Allocator>
      flat_multimap(from_range_t, R&& rg, const key_compare& comp, const Allocator& a);

    template<class InputIterator>
      flat_multimap(sorted_equivalent_t s, InputIterator first, InputIterator last,
                    const key_compare& comp = key_compare())
        : c(), compare(comp) { insert(s, first, last); }
    template<class InputIterator, class Allocator>
      flat_multimap(sorted_equivalent_t, InputIterator first, InputIterator last,
                    const key_compare& comp, const Allocator& a);
    template<class InputIterator, class Allocator>
      flat_multimap(sorted_equivalent_t, InputIterator first, InputIterator last,
                    const Allocator& a);

    flat_multimap(initializer_list<value_type> il,
                  const key_compare& comp = key_compare())
        : flat_multimap(il.begin(), il.end(), comp) { }
    template<class Allocator>
      flat_multimap(initializer_list<value_type> il,
                    const key_compare& comp, const Allocator& a);
    template<class Allocator>
      flat_multimap(initializer_list<value_type> il, const Allocator& a);

    flat_multimap(sorted_equivalent_t s, initializer_list<value_type> il,
                  const key_compare& comp = key_compare())
        : flat_multimap(s, il.begin(), il.end(), comp) { }
    template<class Allocator>
      flat_multimap(sorted_equivalent_t, initializer_list<value_type> il,
                    const key_compare& comp, const Allocator& a);
    template<class Allocator>
      flat_multimap(sorted_equivalent_t, initializer_list<value_type> il,
                    const Allocator& a);

    flat_multimap& operator=(initializer_list<value_type> il);

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

    template<class P> iterator insert(P&& x);
    template<class P>
      iterator insert(const_iterator position, P&&);
    template<class InputIterator>
      void insert(InputIterator first, InputIterator last);
    template<class InputIterator>
      void insert(sorted_equivalent_t, InputIterator first, InputIterator last);
    template<container-compatible-range<value_type> R>
      void insert_range(R&& rg);

    void insert(initializer_list<value_type> il)
      { insert(il.begin(), il.end()); }
    void insert(sorted_equivalent_t s, initializer_list<value_type> il)
      { insert(s, il.begin(), il.end()); }

    containers extract() &&;
    void replace(key_container_type&& key_cont, mapped_container_type&& mapped_cont);

    iterator erase(iterator position);
    iterator erase(const_iterator position);
    size_type erase(const key_type& x);
    template<class K> size_type erase(K&& x);
    iterator erase(const_iterator first, const_iterator last);

    void swap(flat_multimap&) noexcept;
    void clear() noexcept;

    // observers
    key_compare key_comp() const;
    value_compare value_comp() const;

    const key_container_type& keys() const noexcept { return c.keys; }
    const mapped_container_type& values() const noexcept { return c.values; }

    // map operations
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

    friend bool operator==(const flat_multimap& x, const flat_multimap& y);

    friend /*synth-three-way-result*/<value_type>
      operator<=>(const flat_multimap& x, const flat_multimap& y);

    friend void swap(flat_multimap& x, flat_multimap& y) noexcept
      { x.swap(y); }

  private:
    containers c;               // exposition only
    key_compare compare;        // exposition only
  };

  template<class KeyContainer, class MappedContainer>
    flat_multimap(KeyContainer, MappedContainer)
      -> flat_multimap<typename KeyContainer::value_type,
                       typename MappedContainer::value_type,
                       less<typename KeyContainer::value_type>,
                       KeyContainer, MappedContainer>;

  template<class KeyContainer, class MappedContainer, class Allocator>
    flat_multimap(KeyContainer, MappedContainer, Allocator)
      -> flat_multimap<typename KeyContainer::value_type,
                       typename MappedContainer::value_type,
                       less<typename KeyContainer::value_type>,
                       KeyContainer, MappedContainer>;

  template<class KeyContainer, class MappedContainer>
    flat_multimap(sorted_equivalent_t, KeyContainer, MappedContainer)
      -> flat_multimap<typename KeyContainer::value_type,
                       typename MappedContainer::value_type,
                       less<typename KeyContainer::value_type>,
                       KeyContainer, MappedContainer>;

  template<class KeyContainer, class MappedContainer, class Allocator>
    flat_multimap(sorted_equivalent_t, KeyContainer, MappedContainer, Allocator)
      -> flat_multimap<typename KeyContainer::value_type,
                       typename MappedContainer::value_type,
                       less<typename KeyContainer::value_type>,
                       KeyContainer, MappedContainer>;

  template<class InputIterator, class Compare = less</*iter-key-type*/<InputIterator>>>
    flat_multimap(InputIterator, InputIterator, Compare = Compare())
      -> flat_multimap</*iter-key-type*/<InputIterator>,
                       /*iter-mapped-type*/<InputIterator>, Compare>;

  template<class InputIterator, class Compare = less</*iter-key-type*/<InputIterator>>>
    flat_multimap(sorted_equivalent_t, InputIterator, InputIterator, Compare = Compare())
      -> flat_multimap</*iter-key-type*/<InputIterator>,
                       /*iter-mapped-type*/<InputIterator>, Compare>;

  template<ranges::input_range R, class Compare = less</*range-key-type*/<R>>,
           class Allocator>
    flat_multimap(from_range_t, R&&, Compare = Compare(), Allocator = Allocator())
      -> flat_multimap</*range-key-type*/<R>, /*range-mapped-type*/<R>, Compare>;

  template<ranges::input_range R, class Allocator>
    flat_multimap(from_range_t, R&&, Allocator)
      -> flat_multimap</*range-key-type*/<R>, /*range-mapped-type*/<R>>;

  template<class Key, class T, class Compare = less<Key>>
    flat_multimap(initializer_list<pair<Key, T>>, Compare = Compare())
      -> flat_multimap<Key, T, Compare>;

  template<class Key, class T, class Compare = less<Key>>
    flat_multimap(sorted_equivalent_t, initializer_list<pair<Key, T>>,
                  Compare = Compare())
        -> flat_multimap<Key, T, Compare>;

  template<class Key, class T, class Compare, class KeyContainer, class MappedContainer,
            class Allocator>
    struct uses_allocator<flat_multimap<Key, T, Compare, KeyContainer, MappedContainer>,
                          Allocator>
      : bool_constant<uses_allocator_v<KeyContainer, Allocator> &&
                      uses_allocator_v<MappedContainer, Allocator>> { };
}
```

### References

- C++23 standard (ISO/IEC 14882:2023):

---
*Source: https://en.cppreference.com/w/cpp/header/flat_map*
