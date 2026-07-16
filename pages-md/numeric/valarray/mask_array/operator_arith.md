# std::mask_array<T>::operator+=,-=,*=,/=,%=,&=,|=,^=,<<=,>>=

```cpp
void operator+=( const std::valarray<T>& other ) const;
void operator-=( const std::valarray<T>& other ) const;
void operator*=( const std::valarray<T>& other ) const;
void operator/=( const std::valarray<T>& other ) const;
void operator%=( const std::valarray<T>& other ) const;
void operator&=( const std::valarray<T>& other ) const;
void operator|=( const std::valarray<T>& other ) const;
void operator^=( const std::valarray<T>& other ) const;
void operator<<=( const std::valarray<T>& other ) const;
void operator>>=( const std::valarray<T>& other ) const;
```

Applies the corresponding operation to the referred elements and the elements of
`other`.

### Parameters

- **other** — argument array to retrieve the values from

### Return value

(none)

### Exceptions

May throw implementation-defined exceptions.

### Example

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/mask_array/operator_arith*
